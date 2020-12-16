---
title: "Testcontainers Part 1: An Introduction to a new way of integration testing"
date: 2020-12-15T10:06:35+05:30
draft: true
---

Note: This post does not cover code examples for Testcontainers. That will be
part of part 2.

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

What we did was - in our local machine we had a Postgres Server running. It
used to have two databases - one for the actual web application to run, another
database called the test database to run the integration tests. I think this is
a pretty classic method.

So, that's one way - have the actual external component running in your local.
The integration tests run against some sort of test area / namespace and you
are good to go.

Now, we spoke about how to run an external component and run integration test
with it in our local machine. What about Continuous Integration (CI) pipeline?
If you have one that is. I would recommend you to have one! :)

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

If that seems like a simple and plausible solution and you are probably already
using it and are happy with it. I think you can just move on to a different
blog post :) I'm serious, because things are going to get a bit complicated
from here.

Now, if you are curious to know more and also other ways to run integration
tests, you can read on.

There's one gotcha in the above solution. Sometimes Continuous Integration (CI)
systems run multiple pipelines for multiple commits at the same time - for
various reasons. If that's possible in your case, you need to understand that
the test database is shared by the integration tests and are probably running
in parallel in different pipelines. You just need to ensure that there are no
problems because of this. I can think of one problem - if the integration test
data is static, then it's possible that the tests running in parallel might try
to put the same data in the test database. For example if there are any unique
value constraints in the table, that's going to have failures in a perfectly
fine scenario with correct code and test code. All just because of the test
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

Yet another thing you can do is get help from your CI system ;) I know a CI
system that have some cool features to help you with this exact requirement.
The GitLab CI/CD system has a feature called "Services" which can be used to
run any application using a Docker image. So, in case of your integration
tests, you can first run the external component using Docker with the help of
GitLab CI and then you can run your integration tests. You can find more
information about Services in the
[GitLab CI Services Examples Doc](https://docs.gitlab.com/ee/ci/services/README.html)

Check if your CI system can also help you with a similar feature. I also know
some CI systems support plugins, for example Jenkins supports plugins or
extensions. Check if an extension can help you with this requirement.

Such a solution also means that your CI should support such a feature in some
way or the other. Usually such features use something like Docker to help you
run the external component. Maybe it can also be run with a VM too. It really
depends on the feature. But VMs could be an overkill and might take time to spin
up too. But there are some modern day virtualization options which can be
pretty good. For example I have heard a lot of good things about
[firecracker](https://github.com/firecracker-microvm/firecracker).

Such a CI feature means that you can get your external component on-demand.
Usually that's how it is. If it's not on-demand, it's no different from running
an always running external component. In GitLab CI services feature, service is
about an on-demand component using Docker containers. It also means that you
need to have a Docker engine and have support for that in your CI system for
Docker integration probably. At least that's how it is in GitLab CI.

I usually think of Docker when it comes to on-demand components. On-demand VMs
might be an overkill depending on the situation, but it's not impossible to do
it. If your situation demands on-demand VMs over on-demand containers, sure, go
ahead.

Also, on-demand components reminds of serverless architecture / serverless
functions. Since it's on-demand, you might not be running your components all
the time - which might save you some cost in terms of infrastructure. ;) But if
you think about it, you could also share your app and test infrastructure, for
example have test DB in the Postgres Server that has the application DB for your
development environment. It all depends on your situation. Maybe you can't
share infrastructure for your application and test, then it might be an extra
cost if you are always running test infrastructure. So that's an angle to look
at too - cost of resources for your test infrastructure setup. Some companies
maybe able to shell out money for infrastructure than break their heads at
solving such problems with code and scripts or extra technologies like Docker
🤷‍♂️

On-demand components also mean that your component is going to take some time
to start up and run. Your tests need to wait for that to happen. If you have an
always running component, your tests won't have any sort of wait time. That's
also something to note. This wait time really depends on the component and what
you are using to run the component (Containers, VM etc)

Now that we have spoken about so many different ways, let's also talk about one
more way before we go into the Testcontainers topic.

If your CI system does not provide any sort of built in Docker integration or
any other feature to run on-demand components, you can still use Docker or even
VM if that's what you need, and run on-demand components. How? We are going to
try to replicate what GitLab CI's services feature might be doing. I'll show
an example with Docker. You can follow a similar strategy for VMs.

You would have some sort of command to run tests in your CI configuration

```bash
$ command-to-run-test
```

What you can do is, use Docker to run containers before the test command and
stop the container once you are done

```bash
$ run-docker-container-script
$ command-to-run-test
$ stop-docker-container-script
```

One thing to note is, you might have to expose ports from your Docker container
to the host machine. Also, ensure that there is no sort of clashing among the
Docker containers. For example, if you name your containers with custom static
names, and if multiple pipelines run using same Docker engine, then there will
be container name clash. Also, if you are exposing ports to the host machine
running the test command, if multiple pipelines are running Docker containers
in the same machine, then port clashing might happen. These are some things to
take care of.

With some amount of scripting, and sorting out any possible clashing issues
depending on your situation, you should be good to go to use Docker. You still
need Docker engine. This was using Docker. If you are using VMs, you can adopt
a similar strategy probably. I'm not an expert on VMs, so I'm not going to go
there.

Also, I kept mentioning Docker. If you are going to run containers, you can also
choose a different Container Runtime apart from Docker. Docker is just one
famous option out there. But if your CI or some CI plugin is providing some
feature - I guess you just have to use whatever runtime it has to offer.

One thing about Docker or any container runtime or virtualization solutions is,
you could have some issues with adoption. The below can be a problem for
you. I'm taking Docker for example

- You have Docker but can't access the Docker engine directly for some reason
- You don't have Docker in your CI environment. You don't have control over
  what can be and cannot be installed in the CI environment. For example, you
  can't install Docker and related things for it.

You can think of similar problems with any other container runtime or even
virtualization solutions. It's possible that you don't have control over things
because usually only some admins have access and control in infrastructure
related stuff. You might have to see how to cross all the hurdles, and take
decisions and get permissions to be able to implement these solutions. Or
rethink how else you want to solve this problem easily.

Now we have spoken about a lot of different options and the benefits and
possible pitfalls in different options. I guess we have also felt the simplicity
and complexity in the different options. We might have also realized the cost
of the different options and this is cost in different angles - implementation
and maintenance cost of different solutions, resource cost.

About pitfalls, maybe these are real pitfalls for some, and probably not a
pitfall at all for others, which is a great thing. Same goes for complexity and
cost. It really depends on your situation as I always used Postgres DB as an
example. If any one of the above solutions is already helping you or will help
you, you can move on and use it and keep using it.

If you are curious to find out what Testcontainers is about, it starts just now.
;)

### Testcontainers

[Testcontainers](https://www.testcontainers.org/) is a library that helps you
run on-demand components in Docker containers for your integration tests.
There's a Java library and there are libraries in other languages too, like
Golang, Rust, Scala, Python, Js (Node environment), C# (Dot Net). I have
personally used only the Java library.

I have noticed that [Testcontainers Java site](https://www.testcontainers.org/)
currently mentions only about support for Docker container runtime. So
I'm going to focus only on Docker in this post. If you are looking for any
other container runtime for some reason, I would recommend you to explore it
out :)

Testcontainers is Open Source. You can find all their code on GitHub under the
[Testcontainers Github Organization](https://github.com/testcontainers)

Testcontainers library simply uses Docker API (using Docker libraries) and
wraps around it and provides a simpler interface for you to run on-demand
components for your tests.

Testcontainers also has integration with some Testing frameworks. For example
in Java, it integrate well with JUnit.

Using Testcontainers is kind of like running a script to run Docker containers
just before you run the tests. One difference is - in Testcontainers, the
containers are started only when your integration tests need them, the
containers are not started when your unit tests are running. This still depends
on how you write test code and use Testcontainers. Even with Testcontainers,
there is still the time taken for the container to start up - so there is still
little waiting, but you can't avoid it, as it's all on-demand components.
Testcontainers also runs a container of it's own for some sort of container
management. Testcontainers also takes care of the cleanup of containers when
all the tests are done running.

Another cool thing about Testcontainers is that it has some APIs to run some
pretty popular components, like Postgres, Redis etc along with some setup
configuration, all using code! You can also run any custom component or any
component that's not present by default in Testcontainers. The component just
needs a Docker image and you are good to go :) Testcontainers just makes things
easy for some of the popular components - but that's still a small list of
components only, so you might have to write some more code at times for running
your component or find any Open Source libraries that integrate with
Testcontainers and provide your component.

Testcontainers also helps with some networking. For example, you don't have to
worry about ports clashing, or hostnames. Testcontainers helps you expose
container ports at random host ports and provides the value to you
programmatically. It also provides the hostname programmatically.

There are some more features that Testcontainers provides, personally I have
only used these. These are also some notable ones. I have also used
programmatic volume mounting feature using Testcontainers :)

You can find all the features of Testcontainers Java library in the
[Testcontainers website](https://www.testcontainers.org/). For other languages,
you can find the links to docs in the respective library repositories. For
example, for Testcontainers Python -
[Repository](https://github.com/testcontainers/testcontainers-python),
[Package](https://pypi.org/project/testcontainers/),
[Docs](https://testcontainers-python.readthedocs.io/en/latest/?badge=latest).
You can find the language library's source code repository from the
[Testcontainers GitHub Organization](https://github.com/testcontainers)

If you think about it, you could use Testcontainers library to simply run
containers in your main code instead of just tests, in case you need that.
But I guess Testcontainers is particularly built for tests with integration for
testing frameworks. No wonder the name has "test" in it :) Also, if you need to
just run Docker containers and not for your tests, you could also try using a
simple Docker library based on your language instead of Testcontainers. Each
may have it's pros and cons, but particular - each is built with different use
cases in mind :)

Finally, you can use Testcontainers in your local and in your CI, as long as
you have access to Docker in both the environments.

### Some Gotchas about Testcontainers

Now, I have mentioned a lot of good things about Testcontainers. But usually
it's not always rainbows and unicorns. If I wear my inspecting lens, I can see
some possible cons, which may or may not be significant to you.

With the decision of choosing Testcontainers, you couple your code with a
particular platform to run tests or any anything that needs containers and
uses the Testcontainers library. So, anyone using your codebase needs to have
Docker installed in their local machine or have access to a remote Docker
engine.

If you see the solutions we were exploring prior to Testcontainers, all of them
had only external changes - different test DB or test component, services or
scripts to run Docker container to run on-demand components. No code or test
code change. The tests can run in any environment - as long as it has access to
the component. Unlike Testcontainers where the test code is aware of the
environment it needs to run the tests - it needs Docker, as it's running
containers through test code, before running tests.

You could overcome this con though. You can decouple your test code from the
Testcontainers library usage in such a way that you can use some sort of
configuration (feature toggle) to enable or disable using Testcontainers and
Docker to run external components for the tests. It can be a bit of an effort
to get the extra benefit that your users can still run tests with just change
in test configuration without using Testcontainers and Docker, but instead
running the external components themselves in any way they want.

Another thing is, you have to be careful about when you are using Testcontainers
in your code. If you abuse the usage of Testcontainers - even when it's not
necessary, for example, using it in unit tests, using it in tests where the
code for the external components are mocked, then you are going to be running
external components simply - which won't even be used, and it will also add up
to the time taken for running your tests. From what I know, no one likes to
run tests for a long time. People like speed :) So, you gotta beware of where
you use Testcontainers and write test code properly. Use it wisely. Integration
tests - tests running external components - are slow.

### Adoption of Testcontainers

I would recommend you to ask yourself some questions around what are the
different benefits you are getting from Testcontainers and what's the cost of
introducing it, and any hassles you see from your perspective.

Also, another thing about Testcontainers is it would be an additional test
library for your project and probably a bit of a significant one. Of course
your team should agree to using it. This applies to using any solution and not
just Testcontainers. So, talk to your team. If your team is okay with
Testcontainers and sees the benefits it offers and it seems like a
feasible thing, then go ahead and use it :)
