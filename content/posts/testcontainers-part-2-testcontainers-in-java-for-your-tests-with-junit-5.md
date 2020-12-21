---
title: "Testcontainers Part 2: Testcontainers in Java for your tests with Junit 5"
date: 2020-12-20T13:04:35+05:30
---

Testcontainers is a library that helps you run Docker containers to run
components for your integration tests. There's a Java library and there are
libraries in other languages too.

I wrote a [part 1 blog post](https://karuppiah7890.github.io/blog/posts/testcontainers-part-1-an-introduction-to-a-new-way-of-integration-testing/)
about integration tests and an introduction to Testcontainers. It didn't have
any code examples though.

A lot of the docs on the websites are really good for you to get started. I'm
still writing this and upcoming posts to give a bit more details based on the
experience from using Testcontainers in our project. If you are new to
Testcontainers, I'm sure you will have at least a few takeaways from this blog
post :)

In this post I'll be focussing on some code examples with some sample use cases.
You can find all the code examples can be found in my
[testcontainers-demo GitHub repo](https://github.com/karuppiah7890/testcontainers-demo).
This post will only focus on Java language and just plain Java. I'll cover how
to use Testcontainers particularly with Spring Boot framework in another
article, as I know many use Spring Boot and I use it too. It's a pretty
common use case.

Now, let's get started! Testcontainers Java is an Open Source library.

[GitHub source code repository for Testcontainers Java](https://github.com/testcontainers/testcontainers-java/)

[Testcontainers Java website](https://testcontainers.org)

Personally, Testcontainers seems like an easy to use library for me. It can
easily integrate with your test code if you use Junit, as it has integration
for Junit 4 and Junit 5. So, if you are using those, which you probably are,
good for you! A full list of integrations with testing frameworks can be found
in the Testcontainers website.

I'll be showing code examples with Junit 5 only though.

Testcontainers comes built-in with a lot of default set of components which you
can run easily. For example - PostgreSQL, Kafka, Nginx etc. Testcontainers
calls this as "Modules". Let's start by using one such module

## Using Built-in Modules from Testcontainers to run components

Let's say I want to run a PostgreSQL while running my integration tests so that
I can test my code which accesses PostgreSQL. How would I go about doing that?

Since may not be using PostgreSQL, I don't want to get into the details of how
I wrote code related to it. So I'll just give an overview and show how I test
it :)

Let me first write some code which accesses PostgreSQL. Just for the sake of an
example, I'm going to be running a simple SQL query which can run in any
database

```sql
select 1 as some_value;
```

Let's see the Java code for this.

```java
package testcontainers.demo;

import java.sql.*;
import java.util.Properties;

public class SqlDataStore {
    String jdbcUrl;
    private final String username;
    private final String password;

    public SqlDataStore(String jdbcUrl, String username, String password) {
        this.jdbcUrl = jdbcUrl;
        this.username = username;
        this.password = password;
    }

    int runDummyQuery() throws SQLException {
        Properties props = new Properties();
        props.setProperty("user", username);
        props.setProperty("password", password);

        try (Connection conn = DriverManager.getConnection(jdbcUrl, props);
             Statement st = conn.createStatement();) {
            ResultSet rs = st.executeQuery("SELECT 1 as some_value");
            if (rs.next()) {
                return rs.getInt(1);
            }
            rs.close();
        }
        return 0;
    }

}
```

Let's also see the test code I wrote for this, assuming that Postgres Server
is running

```java
package testcontainers.demo;

import org.junit.jupiter.api.Test;

import java.sql.SQLException;

import static org.junit.jupiter.api.Assertions.assertEquals;

class SqlDataStoreTest {
    @Test
    void runDummyQuery() throws SQLException {
        SqlDataStore sqlDataStore =
                new SqlDataStore("jdbc:postgresql://localhost/postgres", "karuppiahn", "");
        int result = sqlDataStore.runDummyQuery();
        assertEquals(1, result);
    }
}
```

I had Postgres Server running in my local using
[PostgresApp](https://postgresapp.com/) for Mac OS which had a default
database named `postgres`. So the above test passed.

Also notice how I'm able to inject the configuration for which DB to connect to
using the JDBC URL / Connection URL. This is useful so that I can have my DB
running anywhere and still the run the test. Here the connection URL is hard
coded but it could also be fetched from a config file / from command line
arguments / environment variables. This also helps you to have different
database for test and the main application. In the test, you pass different
config and connect to different database.

Now, how do I run the same test using Testcontainers? By running Postgres in
a container.

To get started, you need the Testcontainers core library. You can find it here
on Maven Central

https://search.maven.org/artifact/org.testcontainers/testcontainers

You can get the latest version. As of this writing, the latest version is
`1.15.1`

Apart from the core library, you also need a library for the Junit 5
integration. You can find that here on Maven Central

https://search.maven.org/artifact/org.testcontainers/junit-jupiter

And to run Postgres in a container using Testcontainers, you also need the
Postgres Testcontainers Module which you can find here on Maven Central

https://search.maven.org/artifact/org.testcontainers/postgresql

The versions are the same across all the libraries, depending upon the core
library version. Since I'm using gradle, I'm adding this to the project

```groovy
def testcontainers_version = "1.15.1"

testImplementation "org.testcontainers:testcontainers:$testcontainers_version"
testImplementation "org.testcontainers:junit-jupiter:$testcontainers_version"
testImplementation "org.testcontainers:postgresql:$testcontainers_version"
```

With that, let's get to the code now! :) The source code remains the same. Only
the test code is going to change.

```java
package testcontainers.demo;

import org.junit.jupiter.api.Test;
import org.testcontainers.containers.PostgreSQLContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

import java.sql.SQLException;

import static org.junit.jupiter.api.Assertions.assertEquals;

@Testcontainers
class SqlDataStoreTest {

    @Container
    PostgreSQLContainer postgreSQLContainer =
            new PostgreSQLContainer("postgres:13-alpine");

    @Test
    void runDummyQuery() throws SQLException {
        SqlDataStore sqlDataStore =
                new SqlDataStore(postgreSQLContainer.getJdbcUrl(),
                        postgreSQLContainer.getUsername(),
                        postgreSQLContainer.getPassword());
        int result = sqlDataStore.runDummyQuery();
        assertEquals(1, result);
    }
}
```

Ensure you have Docker Engine up and running. Check it out by running some
`docker` command like `docker ps`. After ensuring that Docker is running, you
can then run the tests and see how it runs and passes.

If Docker is not running some weird short error will popup and with full
stacktrace it will be clear that the reason for failure was that the Docker
Engine was not running. So, ensure that your Docker Engine is running.

Now, let's get to the code. You can see how we have used some annotations from
Testcontainers library. What we have done over here is - mentioned the
`@TestContainers` annotation which is a JUnit Jupiter Extension which helps
with automatic startup and stop of containers.

`@TestContainers` and `@Container` annotations go hand in hand.
`@TestContainer` manages the containers annotated with `@Container`. What I
mean by managing is - starting the container, stopping the container. When does
this happen? This depends on how you declare the container variable - instance
or static variable.

If it's static, a single container is started and shared across all test
methods and then stopped.

If it's an instance variable, for each test method, a new container is created
for the test execution and once the test method is done executing, the
container is stopped and cleaned up.

You can find a lot of details in the documentation of these annotations :)

What's happening over here is - before our test runs, a Postgres Docker
container is started, and then the tests are run.

The usual assumption is that - since your tests are integration tests and need
some components, for example Postgres in this case, the containers running these
components must be started before the test runs so that the test execution can
access the component.

Also, you can see how easy it is to run a Postgres Container. I just had to
use the Class provided by the Testcontainers Postgres Module, and provide a
Docker image name. Actually it also has some default Docker image name, but it's
advised to explicitly provide a value. In this case, I'm using PostgreSQL v13.

I have also used an alpine OS based Docker image to keep the Docker image size
small so that not much disk space is needed to store it. Also, it makes it easy
to download it fast in case the Docker image is not present in the Docker
engine.

I would recommend you to use Docker images which are small size and still work
and get things done. It's a pretty standard practice and recommendation :)

Apart from Docker image size, you can also ensure that the software in the
Docker image has a good startup time to start the software when the container
starts. In this case it's the startup time of a Postgres Server. I'm mentioning
this as your tests depend on this Docker container. Only if your Docker
container's startup time is fast, only then your integration tests can run soon!
Everyone wants their tests to run soon. Also, it would be good to have fast
shutdown time too, so that the container can be cleaned up soon. In that front,
Postgres Servers start and stop pretty fast! :)

When you run the test with Testcontainers, you can also check when containers
are created and what containers are created. You will notice that apart from the
components you need, in this case it's Postgres, Testcontainers will also run
another container using an image called
[`testcontainers/ryuk`](https://hub.docker.com/r/testcontainers/ryuk) for some
container management purposes. So, you will notice two Docker containers - one
for Postgres, another for ryuk. You can check this out by running a command like
the below when running the tests

```bash
$ watch docker ps
```

You have seen the basic code for Testcontainers and also the running of a test
with it. Another part of the code we didn't discuss is - the DB config. The
container variable has getter methods to get the config like username, password
and even the JDBC URL which contains the hostname, port and even the DB name.
So, it's pretty easy to inject that config to your test code!

Testcontainers takes care of exposing the container at a random port in the
host machine and also gives you the hostname in a programmatic manner, so you
don't have to hard code it as `localhost`, as sometimes, that might not be the
right value depending on the environment - local machine, Continuous Integration
pipeline (CI).

In the case of Postgres container, this is all hidden behind
`postgreSQLContainer.getJdbcUrl()`, which contains the hostname and the random
port. The random port number helps with no clashing of port numbers with any
other existing applications running in the host machine. It has few more
networking features, which you can find in the Testcontainers website in the
[networking section](https://www.testcontainers.org/features/networking/), and
we will also look at some in the next section.

If you want to run the DB with some specific config - for example say with a
specific username and password, or with a specific DB name, you can do it like
this

```java
@Container
PostgreSQLContainer postgreSQLContainer =
    new PostgreSQLContainer("postgres:13-alpine")
        .withUsername("karuppiah")
        .withPassword("password")
        .withDatabaseName("random");
```

Of course you can also use different Postgres version. You can also use
Initialization Scripts - SQL scripts, which will run when the DB starts, like
this

```java
@Container
PostgreSQLContainer<?> postgreSQLContainer =
    new PostgreSQLContainer<>("postgres:13-alpine")
            .withUsername("karuppiah")
            .withPassword("password")
            .withDatabaseName("random")
            .withInitScript("scripts/initialize-stuff.sql");
```

This was just one example with Postgres component. Similar to this, depending
on the component, you can find similar options to configure the component with
nice methods.

## Running components using Testcontainers without built-in Modules

If Testcontainers provides the component you need in the form of a built-in
Module, I would recommend you to use it. That way you can avoid writing too
much code. Most of the time it should be enough for basic use cases.

Sometimes the component you want to run might not be available in
Testcontainers as a Module, or the features provided by the built-in Module
may not be enough for you. In this case - you can either look for Modules
provided by external libraries to get something up and running soon and not
having to maintain any code. But if you can't find that too, then you have to
write your own. Writing your own Module is not that hard ;) :D Let's see how
to do it!

The Testcontainers library has a Generic Container Module concept, which can run
any Docker Image. Testcontainers also helps you with the networking.

Let's say I have some code which connects to Redis and gets some data. Now I
want to write tests to ensure that the integration with Redis works using
integration tests.

Let's see how the Java code looks like for this Redis code.

```java
package testcontainers.demo;

import redis.clients.jedis.Jedis;

public class RedisBackedCache {
    private final Jedis jedis;

    public RedisBackedCache(String redisHost, int port) {
        this.jedis = new Jedis(redisHost, port);
    }

    public String put(String key, String value) {
        return jedis.set(key, value);
    }

    public String get(String key) {
        return jedis.get(key);
    }
}
```

Now, how do I write a test for this using Testcontainers? I can write it like
this

```java
package testcontainers.demo;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.testcontainers.containers.GenericContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

import static org.junit.jupiter.api.Assertions.assertEquals;

@Testcontainers
class RedisBackedCacheIntTestStep {
    @Container
    public GenericContainer redis = new GenericContainer("redis:5.0.3-alpine")
            .withExposedPorts(6379);
    private RedisBackedCache underTest;

    @BeforeEach
    public void setUp() {
        String address = redis.getHost();
        Integer port = redis.getFirstMappedPort();

        // Now we have an address and port for Redis, no matter where it is running
        underTest = new RedisBackedCache(address, port);
    }

    @Test
    void testSimplePutAndGet() {
        underTest.put("test", "example");

        String retrieved = underTest.get("test");
        assertEquals("example", retrieved);
    }
}
```

You can see how Redis Docker container is used by using `GenericContainer`. We
are still using the same annotations we used before. Here we also have some
setup code in `@BeforeEach` instead of the test method itself, which is a bit
different from the setup code we saw previously, but you could do it
differently too.

If you read the code, it's pretty straightforward. You can see how we have
mentioned a Docker image name for the redis Docker image, and exposed ports
using Testcontainers easy to use methods for networking. The ports that I expose
are ports which the application in the container is using, which is `6379` .
In the setup code you can see how we get the port for the redis connection
config. This port is probably different from `6379`. Again, Testcontainers
chooses random ports from the host machine and exposes the container through
those ports. Here we say `getFirstMappedPort` as the Generic Container can be
exposing multiple ports, in this case it's only one. If you have multiple
exposed ports, to get the corresponding host port, you can use `getMappedPort`.
For example, in this case it would be

```java
redis.getMappedPort(6379)
```

There are more things you can do with all the features I mentioned, like
Networking. You can even do volume mounts. You can check more about what
features you need and what Testcontainers provides. Most probably the features
you need would be present, unless it's a very rare use case.

Now, we have seen how you can use `GenericContainer` to run any kind of
application inside Docker containers if there's a Docker image for it. Now,
would you do all the Testcontainers setup in each and every test file that you
write? In case you need it among multiple test files. How else can you write
the test in such a way that the Testcontainers test setup code is not repeated
everywhere and is not a hassle and is not bloating up and interfering
readability in the test?

Usually some people might be okay with the duplication in tests. In which case
you can leave the test as is. I'm still going to mention how you can think about
different ideas to use Testcontainers by keeping the Testcontainers setup
away from test code.

## Keeping Testcontainers setup in a separate class

If you have some sort of big setup, you can move the Testcontainers setup to a
separate class and have code like this

```java
package testcontainers.demo;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class RedisBackedCacheIntTestStepTestSuite extends RedisContainerTestSuite {
    private RedisBackedCache underTest;

    @BeforeEach
    public void setUp() {
        String address = redis.getHost();
        Integer port = redis.getFirstMappedPort();

        // Now we have an address and port for Redis, no matter where it is running
        underTest = new RedisBackedCache(address, port);
    }

    @Test
    void testSimplePutAndGet() {
        underTest.put("test", "example");

        String retrieved = underTest.get("test");
        assertEquals("example", retrieved);
    }
}
```

```java
package testcontainers.demo;

import org.testcontainers.containers.GenericContainer;
import org.testcontainers.junit.jupiter.Container;
import org.testcontainers.junit.jupiter.Testcontainers;

@Testcontainers
public class RedisContainerTestSuite {
    @Container
    public GenericContainer redis = new GenericContainer("redis:5.0.3-alpine")
            .withExposedPorts(6379);
}
```

The tests will still work fine :)

## Using Testcontainers with JUnit Extensions

I usually try to avoid using inheritance whenever it's not needed, and try to
prefer composition. Also, Java can only support inheriting from one class, so,
I try to use it properly. In this case, a "Test is a Redis Container Test
Suite" or "Test is a Redis Container Test" might make sense and hence for the
"is a" relationship, people might choose inheritance. But it's a bit
unnecessary.

I'm going to show a simple example of how you can avoid inheritance and still
have most of the test related code in another place and not put it all in the
tests. This will also help you to reuse your Testcontainers test setup code
when you have a bit more than a few lines.

For simplicity, I'm using the Redis example, which is not even a few lines of
code. Ideally, you would do this only when you have lots of lines for setup,
like 10-15 lines or maybe more. Just a ballpark value 😅

As you noticed the title, we are going to look at JUni Extensions. You can use
the concept of
[JUnit Extensions](https://junit.org/junit5/docs/current/user-guide/#extensions)
to your favor to help you with this use case.

You can find more about JUnit Extensions and examples in the official docs, this
is just a code example. The official docs also show an example where the
extension is used to run a web server or a database server! So, I guess it's a
pretty common use case for using Extensions.

The test will now look like this

```java
package testcontainers.demo;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.RegisterExtension;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNull;

public class RedisBackedCacheSharedExtensionBasedSetupTest {
    private RedisBackedCache underTest;

    @RegisterExtension
    static SharedRedisExtension sharedRedisExtension = new SharedRedisExtension();

    @BeforeEach
    public void setUp() {
        String address = sharedRedisExtension.getHost();
        Integer port = sharedRedisExtension.getPort();

        System.out.println(sharedRedisExtension.getContainerID());

        // Now we have an address and port for Redis, no matter where it is running
        underTest = new RedisBackedCache(address, port);
    }

    @Test
    void testSimplePutAndGet() {
        underTest.put("test", "example");

        String retrieved = underTest.get("test");
        assertEquals("example", retrieved);
    }

    @Test
    void anotherTestForSimplePutAndGet() {
        underTest.put("test", "anotherExample");

        String retrieved = underTest.get("test");
        assertEquals("anotherExample", retrieved);
    }
}
```

The extension will look like this

```java
package testcontainers.demo;

import org.junit.jupiter.api.extension.AfterAllCallback;
import org.junit.jupiter.api.extension.BeforeAllCallback;
import org.junit.jupiter.api.extension.Extension;
import org.junit.jupiter.api.extension.ExtensionContext;

public class SharedRedisExtension implements Extension, BeforeAllCallback, AfterAllCallback {
    final RedisContainer redisContainer =
            new RedisContainer("redis:5.0.3-alpine");

    @Override
    public void beforeAll(ExtensionContext context) {
        redisContainer.start();
    }

    @Override
    public void afterAll(ExtensionContext context) throws Exception {
        redisContainer.stop();
    }

    String getHost() {
        return redisContainer.getHost();
    }

    Integer getPort() {
        return redisContainer.getPort();
    }

    String getContainerID() {
        return redisContainer.getContainerId();
    }
}
```

```java
package testcontainers.demo;

import org.testcontainers.containers.GenericContainer;

public class RedisContainer extends GenericContainer<RedisContainer> {
    public static final int REDIS_PORT = 6379;

    public RedisContainer(String dockerImageName) {
        super(dockerImageName);
        addExposedPort(REDIS_PORT);
    }

    Integer getPort() {
        return this.getMappedPort(REDIS_PORT);
    }
}
```

There are multiple ways to use JUnit Extensions. Here I have used it
programmatically. You could also use it with annotations like `@ExtendWith` and
configure it with annotations. But using registering extensions
programmatically helps with getting data like host, port easily. That data is
present inside the extension.

You can also see how the Extension runs before all the tests in this case, and
it runs the container. So, here we are not using any of the Testcontainers
annotations like `@TestContainers` or `@Container` to help with the container
startup and stop lifecycle. Instead we manage the startup of container.

For stop, if we don't stop it using the After All callback, the container will
be stopped only after the JVM stops executing the tests, where Testcontainers
library takes care of cleaning up the containers that weren't stopped. But if
you have many test files with many test methods, many of which might not even
need the container, it's not really advantageous to keep running the containers
in the extension till all of them finish and then let the containers get
cleaned up. So, try to clean it up sooner :)

I have also added a method called `getContainerID` to show the redis container's
ID before each test is run. It's to show that the same redis container is used
in all the tests.

You could do something similar to run a dedicated redis container for each
test method, before each test. For that you need to implement
`BeforeEachCallback` interface in the extension. It all depends on your use
case - share the same container across all the test methods vs dedicated
container for each test method.

Below is an example of how a dedicated redis extension would like to run a
dedicated redis container for each test method.

This is the test code

```java
package testcontainers.demo;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.RegisterExtension;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNull;

public class RedisBackedCacheDedicatedExtensionBasedSetupTest {
    private RedisBackedCache underTest;

    @RegisterExtension
    static DedicatedRedisExtension dedicatedRedisExtension = new DedicatedRedisExtension();

    @BeforeEach
    public void setUp() {
        String address = dedicatedRedisExtension.getHost();
        Integer port = dedicatedRedisExtension.getPort();

        System.out.println(dedicatedRedisExtension.getContainerID());

        // Now we have an address and port for Redis, no matter where it is running
        underTest = new RedisBackedCache(address, port);
    }

    @Test
    void testSimplePutAndGet() {
        underTest.put("test", "example");

        String retrieved = underTest.get("test");
        assertEquals("example", retrieved);
    }

    @Test
    void anotherTestForSimplePutAndGet() {
        String retrieved = underTest.get("test");
        assertNull(retrieved);

        underTest.put("test", "anotherExample");

        retrieved = underTest.get("test");
        assertEquals("anotherExample", retrieved);
    }
}
```

This is the dedicated extension code

```java
package testcontainers.demo;

import org.junit.jupiter.api.extension.AfterEachCallback;
import org.junit.jupiter.api.extension.BeforeEachCallback;
import org.junit.jupiter.api.extension.Extension;
import org.junit.jupiter.api.extension.ExtensionContext;

public class DedicatedRedisExtension implements Extension, BeforeEachCallback, AfterEachCallback {
    final RedisContainer redisContainer =
            new RedisContainer("redis:5.0.3-alpine");

    String getHost() {
        return redisContainer.getHost();
    }

    Integer getPort() {
        return redisContainer.getPort();
    }

    @Override
    public void beforeEach(ExtensionContext context) {
        redisContainer.start();
    }

    public String getContainerID() {
        return redisContainer.getContainerId();
    }

    @Override
    public void afterEach(ExtensionContext context) throws Exception {
        redisContainer.stop();
    }
}
```

Notice how we stop the redis container after each test method using the
`afterEach` method in the extension? If we do not do this, the test methods
will keep using the same redis container. How? The `start` method does get
executed every time a test method runs because of the Before Each call, but
it still uses the same Docker container. Why? It's because the `start` method
does not start any new container if the Docker container associated with the
variable is already present. So, you need to run `stop` to be able to stop the
already running container and then run `start` again to start a new container
using the same variable.

You can also do a lot of customization in this way of using Testcontainers -
with respect to the Container. As we use `GenericContainer` and extend it to
create a `RedisContainer`. If you check the code for `PostgreSQLContainer`, it
will look similar, but with more code 😅 as it needs some more features, and
this is a very simple use case.

So, that's also one way to use TestContainers - using JUnit Extensions :)

## Conclusion

I would recommend JUnit Extensions way along with specific classes for your
containers instead of using `GenericContainer`. This is especially useful when
you have more than just two or three lines of code for Testcontainers setup.
This will also help you use the Extension and the Container across multiple
tests. You will also not be limited to use inheritance - as at times, I have
noticed how I get stuck at times because I can only extend one class, and then
end up doing multi level inheritance and it also complicates things
unnecessarily. This is composition and looks way simpler to me :)

But no one way is a silver bullet for all problems. So choose based on your
problem :)

All code examples can be found on my
[testcontainers-demo GitHub repo](https://github.com/karuppiah7890/testcontainers-demo)
