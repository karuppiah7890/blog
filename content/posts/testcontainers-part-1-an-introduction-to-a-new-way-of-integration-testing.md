---
title: "Testcontainers Part 1: An Introduction to a new way of integration testing"
date: 2020-12-15T10:06:35+05:30
draft: true
---

## TLDR

Sometimes you have some external components in your system - like a data store
or cache or message queue and similar. Some examples of external components
are - Postgres, Redis, Kafka, RabbitMQ.

You might be writing and running integration tests against actual components
to ensure the integration works. To run these actual components - you could do
it in many ways. And you would have to ensure that you can run them in your
local machine and in your Continuous Integration (CI) pipeline too, in case you
have CI.

Testcontainers is a library available in many languages to run Docker
containers easily in a programmatic manner so that your integration tests can
run the components in Docker containers and use them while running the test. It
has it's pros and cons. Read more to understand better :)

## Longer Version

Before we get into the topic of Testcontainers, let's first understand the
requirement we have. The requirement is -

"I have one or more integration tests that needs actual external components to
be running. The integration test(s) need to connect to them and test if the
code works, test if the integration with the external component works."

A classic example is, integration tests with a database. In our project we have
written integration tests with Postgres database. We have also written
integration tests with Kafka.

Now, how would you go about this? I'm sure a lot of people have already solved
this problem in their projects. It's a pretty classic problem.

Let me tell one of the ways that I have seen and worked with. This was
integration tests with Postgres Database in a Ruby on Rails project.

What we did was - in our local we had a Postgres Server running. It used to have
two databases - one for the actual web application to run, another database
called the test database to run the integration tests. I think this is a
pretty classic method.

So, that's one way - have the actual external component running in your local.
The integration tests run against some sort of test area / namespace and you
are good to go.

If that seems like a simple and plausible solution and you are probably already
using it and are happy with it. I think you can just move on to a different
blog post :) I'm serious.

Now, if you are curious to know other ways, you can read on.

Now, we spoke about how to run an external component and run integration test
with it in our local machine. What about Continuous Integration (CI) pipeline?
If you have that is. I would recommend you to have one! :)

One way that I can think of is - again use a solution similar to what we do in
our local machine. I mean, why not?

I'm sure there must be some sort of development environment, kind of like a play
ground for you to deploy your applications and run and test them. It would need
the same external component. So you could use that 🤷‍♂️

Let's take one example for this. Let's say the external component is a Postgres
database. You can create a test database in the development environment Postgres
Server. You can point your integration tests to run against this test database
using application configuration. As simple as that, right? I like this solution
actually. It's similar to the solution we use in our local machine, looks pretty
simple and straight forward.

Just one thing to note is, sometimes Continuous Integration (CI) systems run
multiple pipelines for multiple commits at the same time - for various reasons.
If that's possible in your case, you need to understand that the test database
is shared by the integration tests and are probably running in parallel in
different pipelines. You just need to ensure that there are no problems
because of this. I can think of one problem - if the integration test data is
static, then it's possible that the tests running in parallel might try to put
the same data in the test database. For example if there are any unique value
constraints in the table, that's going to have failures in a perfectly fine
scenario with correct code and test code. All just because of the test
infrastructure setup. This was just one example and that too with Postgres
database. There could be other issues too, and your external component could be
anything. The issues that can pop up depends on the external component and it's
working and how you have written your integration tests. So, it's possible that
you may or may not have such kind of problems.

One thing that you can do to avoid such kind of problems is - write integration
tests in such a way that even if multiple integration tests run in parallel
against the same shared test database, there won't be any problems. For example
by using some random test data, doing proper cleanups etc to avoid any sort
of clashing problems. You can think of a similar solution in case you are using
a different external component and not a database like Postgres. You can write
integration tests based on the attributes of your specific external component
in case you have clashing problems similar to the ones we just spoke about.

Another thing you can do is, you can create a temporary test database with a
random name every time the pipeline runs. The tests can run against that. If
there are multiple tests running in parallel, then there will be multiple
temporary tests databases in the Postgres Server. After the pipeline runs, you
can delete the temporary test database. You might have to do some scripting for
this in your CI pipeline configuration. With this kind of solution, you won't
have to worry about how you have written your integration tests and think if
there would be problems if many of them run in parallel against shared test
database - as we have avoided using shared test database by providing dedicated
temporary test database for each integration test. Again, you can think of a
similar solution in case you are using a different external component, to create
a temporary test area for the integration tests. And you need to do this only
if you have any sort of clashing problems or else you can just chill ;)

Something to note is
