---
title: "Validating Beans and Testing It"
date: 2020-11-22T14:45:36+05:30
---

In my current project, we work with [Spring Boot](https://spring.io/projects/spring-boot). Recently, I was writing a [Data Transfer Object (DTO)](https://en.wikipedia.org/wiki/Data_transfer_object) and we also had to add validations to
the fields. Usually, while doing this, we also add tests to ensure that in the
future we don't remove or change the validations by mistake and instead it's
more intentional as we have to also change the tests.

Some more context - we are writing Web APIs. For testing the DTO validations
that we had added using annotations, what we usually did was - invoke the
controller that was responsible for handling the API request. Invoking through
method or by using Spring MockMvc and writing tests at API level. The method in
the controller took care of validating the DTO. The code looked something like
this -

```java
// UserController.java
package com.example.dtovalidation.controllers;

import com.example.dtovalidation.dtos.UserDto;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import javax.validation.Valid;

@RestController
public class UserController {
    @PostMapping(path = "/users")
    void createUser(@Valid @RequestBody UserDto userDto) {
        // create a user using the UserService
    }
}
```

```java
// UserDto.java
package com.example.dtovalidation.dtos;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.validation.constraints.NotNull;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserDto {
    @NotNull
    private String username;
    @NotNull
    private String email;
}
```

```java
package com.example.dtovalidation.controllers;

import com.example.dtovalidation.dtos.UserDto;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@AutoConfigureMockMvc
@SpringBootTest
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;

    private final ObjectMapper objectMapper = new ObjectMapper();

    @Test
    void givenAnInvalidUserDtoShouldThrowError() throws Exception {
        UserDto userDto = new UserDto(null, "something@gmail.com");
        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(toJson(userDto)))
                .andExpect(status().isBadRequest());
    }

    private String toJson(Object obj) throws JsonProcessingException {
        return objectMapper.writeValueAsString(obj);
    }
}
```

I'm not going to get too much into the controller test code. But if you see
above, there's just one test with one scenario of invalid DTO - username
missing. There could be more cases in your DTO. Here, there's one more - email
missing, and then for valid DTO, we need one test. But you get the drill,
right? Also, if the service methods also take DTOs or some bean, you can do
validation for those beans as well at service level tests.

At some point, I was wondering why these validations tests are to be done at
these levels? Controller, Service. The validation code is present in the DTO /
bean. Why can't I just write a unit test to test if the validations are being
done? To test the Spring integration, I could write one test to check if the
controller and service are validating the bean using `@Valid`. But for the
validation of multiple fields and different scenarios and corner cases, unit
tests are better.

Unit tests of DTO / bean validation in general are lighter with almost, and
hence faster to run. They are also easy to write. You don't need to know
anything about controller, or service or anything. Also, unit tests just need
the subject under test and everything else can be mocked and in this case,
there's nothing to mock too, unlike controller and service tests.

Anyways, let me get to the point. The point is - if you are planning to write
tests to check if the validations in your bean are being done right, then you
could consider unit tests first. And integration tests only to test if the
integration with other pieces works - for example integration with Spring
controller and service.

Now, let's get started. As you can see above in the controller test, that's a
good enough integration test to check if the `@Valid` is present in the method
argument to validate the DTO. For the unit tests, the plan could look something
like this

- Create a DTO
- Have one invalid field and all other fields as valid
- Validate the DTO
- Check if the number of validation errors is one
- Keep repeating this for all fields with a good distinct sample possible
  invalid values

Also have one unit test to test a valid DTO. If you notice, I have put "repeat"
for all field as one of the steps. You could write multiple JUnit test methods
and write the test. In my case, I chose to use Parameterized Test feature of
JUnit and wrote the test for invalid DTOs. I also tried to extract most of the
common code and came up with the below plan for my test

- Create a valid DTO
- Set one of the field with an invalid value. Field and invalid value are the
  test parameters for this parameterized test
- Validate the DTO
- Check if the number of validation errors is one

I referred to this [Baeldung article](https://www.baeldung.com) to check [how to set field value with reflection](https://www.baeldung.com/java-set-private-field-value). There's also one for [Junit Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)

For validating the DTO, which is the tricky part, I searched through a lot only
to finally find the [Jakarta Bean Validation](https://beanvalidation.org/) and
it's [reference implementation](https://beanvalidation.org/2.0/) -
[Hibernate Validator](https://hibernate.org/validator/). Specifically this
guide about [Validating Bean Constraints](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#section-validating-bean-constraints)

You can see the code below

```java
package com.example.dtovalidation.dtos;

import lombok.SneakyThrows;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import javax.validation.ConstraintViolation;
import javax.validation.Validation;
import javax.validation.Validator;
import javax.validation.ValidatorFactory;
import java.lang.reflect.Field;
import java.util.Set;
import java.util.stream.Stream;

import static org.assertj.core.api.Assertions.assertThat;

class UserDtoValidationTest {
    @Test
    void givenValidDto_whenValidated_thenNoValidationError() {
        UserDto userDto = getValidUserDto();

        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        final Validator validator = factory.getValidator();

        Set<ConstraintViolation<UserDto>> constraintViolations =
                validator.validate(userDto);

        assertThat(constraintViolations.size()).isZero();

    }

    static Stream<Arguments> provideFieldAndInvalidValue() {
        return Stream.of(
                Arguments.of("username", null),
                Arguments.of("email", null)
        );
    }

    @SneakyThrows
    @ParameterizedTest
    @MethodSource("provideFieldAndInvalidValue")
    void testInvalidDto(String fieldName, Object invalidValue) {
        UserDto userDto = getValidUserDto();

        Field field = UserDto.class.getDeclaredField(fieldName);
        field.setAccessible(true);
        field.set(userDto, invalidValue);

        ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
        final Validator validator = factory.getValidator();

        Set<ConstraintViolation<UserDto>> constraintViolations =
                validator.validate(userDto);

        assertThat(constraintViolations.size()).isOne();
    }

    private UserDto getValidUserDto() {
        return new UserDto("someName", "something@gmail.com");
    }
}
```

So that's how I wrote DTO validation tests :) I was proud that they were unit
tests and were pretty fast. Having worked with Spring Boot recently, I have
come to realize that it's pretty important to write more of fast unit tests
wherever possible and applicable, as writing integration tests, for example
with Spring Boot, can be slow. And the developer experience (DX) is better when
all the tests in a project run fast. If you have too many integration tests,
then the test run can be too slow. Beware of where you choose to write an
integration test and make sure it's worth it. If the same thing can be tested at
a lower level with unit tests, do it! Not to mention, in some cases, it's very
important to write a few integration tests - like in the case of Spring, to
ensure that your app runs as a whole with proper integration with Spring Boot.
So, a good balance of integration and unit tests is key.

You can find the whole code in the
[dto-validation-demo GitHub repository](https://github.com/karuppiah7890/dto-validation-demo)
