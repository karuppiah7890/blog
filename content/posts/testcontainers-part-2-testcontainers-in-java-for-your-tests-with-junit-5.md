---
title: "Testcontainers Part 2: Testcontainers in Java for your tests with Junit 5"
date: 2020-12-19T12:40:35+05:30
draft: true
---

Testcontainers is a library that helps you run Docker containers to run
components for your integration tests. There's a Java library and there are
libraries in other languages too.

I wrote a [part 1 blog post](https://karuppiah7890.github.io/blog/posts/testcontainers-part-1-an-introduction-to-a-new-way-of-integration-testing/)
about integration tests and an introduction to Testcontainers. It didn't have
any code examples though.

In this post I'll be focussing on some code examples with some sample use cases.
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
Module, I would recommend you to use it to avoid writing too much code. Most of
the time it should be enough for basic use cases.

END

---

Talk about different kinds of containers and also about the ability to create
your own Containers using GenericContainer class

Show examples with Postgres Container and Generic Container

Show examples with Extensions Concept and also with inheritance maybe? Talk
about pros and cons after checking about it! :)
