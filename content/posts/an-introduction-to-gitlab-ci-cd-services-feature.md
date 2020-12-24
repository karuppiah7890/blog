---
title: "An Introduction to GitLab CI/CD Services feature"
date: 2020-12-24T12:33:35+05:30
---

All the code for this blog post is present in the
[demo GitLab repo](https://gitlab.com/karuppiah7890/gitlab-ci-services-demo).  
It has GitLab CI Config and also a demo pipeline which shows how it runs.

Recently I wrote a blog post about
[integration tests and some ways to do it](https://karuppiah7890.github.io/blog/posts/testcontainers-part-1-an-introduction-to-a-new-way-of-integration-testing/). One of the ways talks about using GitLab CI/CD's Services feature. I
realized it deserves a separate blog post, so here it is :D

This blog post is meant to create awareness around this feature of GitLab CI/CD
and also give a basic introduction. Apart from that, the official docs are
pretty good enough :)

So, what is this Services feature and why is it needed? This feature helps you
run any sort of external service when your CI/CD pipeline jobs are running. So,
if you have such a need, then this would be useful. One basic use case that I
know of is - running external services for the purpose of integration testing.

For example, if I have a web application which needs a database, and I have
written integration tests in my project to run tests with an actual database
running, and if I need to run this test in the GitLab CI/CD pipeline, then I
can run the database using the Services feature and then run my integration
tests to use that database

How do you go about using this Services feature? It's pretty straight forward.
I think GitLab CI/CD has tried to make it as simple as possible.

Let's start by seeing an example of how to implement the exact thing I mentioned
above - a test / a job which needs a database. I'll choose PostgreSQL as my
database.

To start off, for using Services feature, the service you need to run should be
available as a Docker Image.

Let's see an example GitLab CI Config with a psql job and PostgreSQL service

```yaml
services:
  - name: postgres:13-alpine
    alias: my-db

variables:
  POSTGRES_PASSWORD: "postgres"

sample-test-job:
  image: governmentpaas/psql
  script:
    - PGPASSWORD=postgres psql -h my-db -U postgres -c "\conninfo"
    - PGPASSWORD=postgres psql -h my-db -U postgres -c "\l"
    - PGPASSWORD=postgres psql -h my-db -U postgres -c "SELECT 1 + 1 as value"
```

I'm trying to run a PostgreSQL using the `postgres:13-alpine` Docker image.
I'm setting the password for the PostgreSQL using the `POSTGRES_PASSWORD`
environment variable - this is based on the Docker image and the application
running in the Docker image. I have not defined other configurations for the
PostgreSQL instance so it's all default values.

I would recommend you to use GitLab CI lint feature to lint and check config
before pushing it to repo. The lint page is specific to the project. For the
demo project, the link is this

https://gitlab.com/karuppiah7890/gitlab-ci-services-demo/-/ci/lint

If you use the above GitLab CI Config in your project, and run a CI pipeline
with it, it will look like this -

![gitlab-ci-services-feature-pipeline-demo](/blog/img/an-introduction-to-gitlab-ci-cd-services-feature/gitlab-ci-services-feature-pipeline-demo.png "gitlab-ci-services-feature-pipeline-demo")

https://gitlab.com/karuppiah7890/gitlab-ci-services-demo/-/jobs/930732004

Notice how it shows that it's running the service and is waiting for it to come
up. I'm not going to be explaining in detail about how GitLab CI checks if the
service is up or not. Docs mention that it checks the connectivity to ports
exposed by the container. . Not sure how this happens in depth. You can check
the ["How the health check of services works"](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#how-the-health-check-of-services-works)
section in the official docs for more information. But in my case, however the
Docker container runs pretty fast so no problem :)

How does the networking between my job container and the service container
work though? For example, to connect to an external service, I need details like
IP address and Port number of the external service. In this case, we have
access to the hostname with the usage of `alias` config, if we don't use `alias`
config, then we can use an auto-generated hostname based on the image name, in
this case it would be `postgres`. This auto-generation logic is something used
and done by GitLab CI. I highly recommend you to use `alias` if you can, as it's
easier to understand and use. But `alias` feature is only present in some
newer versions.

You can check more about accessing the service at the ["Accessing the services"](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#accessing-the-services) section in the official docs.

The configurations / settings available for services including `alias` is
documented in ["Available settings for services"](https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#available-settings-for-services)

I have put a lot of reference links in the [Reference links](#reference-links)
section for you to check out.

The above was just one example, you can similarly run any such external service
for your use case

## Conclusion

That's all there is about GitLab CI Services feature! :)
I believe that this is a great feature provided by GitLab CI/CD to help with
pretty common use cases like integration testing.

Give it a try if you use GitLab CI/CD. Especially if you are doing integration
testing with GitLab CI/CD, but in a different manner!

## Reference links:

https://docs.gitlab.com/ee/ci/services/README.html

https://docs.gitlab.com/ee/ci/services/postgres.html

A lot of details regarding Services feature and the Docker image used by the
Services feature can be found in this web page

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html

Links to some of the headings have been provided here -

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#what-is-a-service

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#how-services-are-linked-to-the-job

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#how-the-health-check-of-services-works

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#accessing-the-services

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#passing-environment-variables-to-services

https://docs.gitlab.com/ee/ci/docker/using_docker_images.html#available-settings-for-services
