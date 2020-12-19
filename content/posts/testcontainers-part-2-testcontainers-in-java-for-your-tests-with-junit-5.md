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
calls this as "Modules". Let's start by using one such module. Let's say I want
to run a PostgreSQL while running my integration tests so that I can test my
code which accesses PostgreSQL. How would I go about doing that?

Since may not be using PostgreSQL, I don't want to get into the details of how
I wrote code related to it. So I'll just give an overview and show how I test
it :)

Let me first write some code which accesses PostgreSQL. Just for the sake of an
example, I'm going to be running a simple SQL query which can run in any
database

```sql
select 1 as some_value;
```

< Show Java code for running the SQL query using a postgres library >

< Show test code. Show that you need to run postgres to be able to run the test >

< Show that you need to configure the test / code with appropriate config about
the DB so that it connects to the right DB. One should be able to inject any
config from outside into the code to connect to any DB, like some sort test DB
which is different from an App DB >

< Show how you can run with Testcontainers now. Add testcontainers dependency,
one core dependency, one Junit integration dependency, one module dependency
to run Postgres container. Add code for postgres container with testcontainers
annotations for lifecycle stuff - to start container before the test. Get the
config related to the
container. Now, run the test! Ask folks to ensure that Docker is running, using
docker ps or something. Mention that if Docker is not running
some weird error will popup and with stacktrace it will be clear that the reason
for failure was that the Docker environment was not found >

< Show that the postgres can be configured with some options - methods. Like,
database name, username, password. And that different postgres versions can
be used. How to use DockerImageName class to provide image name. And how you
can do more advanced stuff like provide some sort of init scrip -
initialization script >

END

---

Talk about different kinds of containers and also about the ability to create
your own Containers using GenericContainer class

Show examples with Postgres Container and Generic Container

Show examples with Extensions Concept and also with inheritance maybe? Talk
about pros and cons after checking about it! :)
