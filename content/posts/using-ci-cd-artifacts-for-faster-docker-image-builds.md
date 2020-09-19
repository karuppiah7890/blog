---
title: "Using CI/CD Artifacts for Faster Docker Image Builds"
date: 2020-09-19T08:29:02+05:30
---

> Disclaimer: This blog post is very context specific, so please consider your application and it's context, and accordingly see if you can use this strategy.

## Story and the Problem

Recently in our project, we were writing a `Dockerfile` to build a Docker image
for our React App. It didn't have any server side to it, just a static app.
We decided to serve the frontend files (HTML, CSS, Js) using Nginx web server.

We followed the usual strategy to build Docker images. This is how our `Dockerfile`
kind of looked like

```Dockerfile
FROM node:12.12.0-alpine AS build-stage

WORKDIR /app

COPY package.json ./
COPY yarn.lock ./
RUN yarn install

COPY src ./src

RUN yarn build

FROM nginx:1.19-alpine
COPY --from=build-stage /app/build/ /usr/share/nginx/html
COPY conf/nginx-default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

It's a multi stage docker image build. I think you can find similar
`Dockerfile`s in many blog posts and GitHub repos too. This is actually a pretty
good `Dockerfile` actually. Anyone can get your repo and need not install
anything except Docker and be able to build the Docker image.

In our case, we were using this `Dockerfile` to build the Docker image to build
the Docker image in our Jenkins CI/CD pipeline.

I felt that the Docker image build was taking lots of time and increased the
time of the execution of the pipeline as a whole.

It would be interesting to note what we run as part of our Jenkins pipeline.
This is what we run

* Sanity stage (Run things inside in parallel)
    * Audit the dependencies
    * Install dependencies and run tests
* Build - `yarn build` to build a production optimized build
* Build Docker Image
* Push Docker Image to a Docker Image registry

If you notice, we have a `Build` stage in our pipeline - this will help us find
any compilation issues when building the code to get the front end files - 
transpiled, bundled, minified.

You will also notice that we run `yarn build` in our `Dockerfile` as part of
building the image too.

That's weird, isn't it? In the pipeline, we run `yarn build` twice. But why?
Exactly. You don't need to. This is what we felt too!

Let's take a close look at how things are working in this particular case -
* For the `yarn build` to work, the dependencies must be installed. This is
taking place in the pipeline before running our tests.
* `yarn build` is the final thing to run to get the build of our React App -
which is the only thing needed to run the App, along with a web server to serve
the files.
* In the `Dockerfile`, all the commands at the top of the file are done so that
`yarn build` can be run - installing dependencies, copying source code.

## Strategy

So, what can we do to avoid the problem of running `yarn build` twice? We can
just use the output/artifact of `yarn build` from the build stage in the
pipeline in our `Dockerfile`

So, in short, we can get rid of almost everything in our `Dockerfile` and it
boils down to this

```Dockerfile
FROM nginx:1.19-alpine
COPY ./build/ /usr/share/nginx/html
COPY conf/nginx-default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

## Benefits

That's like very small and the Docker image build is very very fast! If you
notice, we are simply copying the `build` directory that was the output/artifact
of the Build stage.

This makes the execution time of the whole pipeline very less - which is pretty
important I think - because a lot of times, developers might be waiting for the
pipeline for different reasons:
- For checking if their PR pipeline succeeds to be able to merge PRs. Or better,
maybe Docker image build can also be removed from such pipelines if it's not
really needed - and if other stages can give a notion that the code works - 
for example - audit, test, build.
- For pulling the latest code from the remote git repo - developers check if the
latest commit on the branch has passed the pipeline with a green status.
- For deploying the code to different environments using CD pipeline, after
pushing code

You can read more about making pipeline faster and it's benefits online :)

## Will this strategy always help?

I have put a disclaimer on the top - this strategy is very context specific.

For example, nodejs based services, ruby based services, python based services,
and few other similar interpreted languages - they require that the source code
be present in the Docker image for it to run at run time. Also, when it comes to
dependencies / modules / packages / libraries, we install only the ones that are
required for running the app in production. In case of your pipeline stages,
you might be installing all dependencies - test related dependencies too, so
that you can run tests in your pipeline. So, these artifacts might not help at all.
You will have to run a lot of steps to do your Docker image build in such cases,
and build artifacts or other pipeline stage artifacts might not be able to help
you.

## Squeeze out the performance

Another thing that can make your Docker image build slow is - the time taken for
the build to even start. This is a common problem for anyone starting out new in
Docker and build Docker images.

The key here is the build context. This is the input that's passed as the
last argument in the Docker build command

```bash
$ docker build -t local-image .
```

The `.` is the build context and it can be anything based on your app and use
case and from where you are running your build command in the shell. Now the
important thing to note is - by default all the contents of the directory
mentioned as build context is sent to the Docker daemon (Docker engine). In our
case, we had the long `Dockerfile` and we also sent our whole project directory
to Docker daemon. Guess how big this build context was when I tried to run it
in my local? It was 400 MB. 400 whopping MB! It was clearly visible when Docker
was slowly sending 400 MB of data to the Docker daemon and showing the progress
in the form of a number which was moving pretty fast but still it took time for
it go from 0 to 400 MB. 

This wasn't visible in our Jenkins pipeline though. Why? What was the issue?
This was because in our local, we have the biiiig `node_modules` directory,
which is getting sent as part of the build context to Docker daemon. This wasn't
visible in the Jenkins pipeline because the `node_modules` directory is ignored
in the `.gitignore` file, so it doesn't reach the Jenkins pipeline.

Docker has this nice feature to ignore files, so that they are not sent to Docker
daemon as part of the build context. This is similar to the `.gitignore` feature,
it's called Docker ignore and the file is `.dockerignore`.

For our case, we can surely ignore `node_modules`. Or we can ignore more - we
can ignore almost everything, except for the files that are needed for the
Docker image build - which in our case is the `build` directory and a `conf`
directory containing some Nginx configuration. So I just went ahead and ignore
**everything** except the `build` and `conf` directories in the `.dockerignore`
file

```
*
*/
!/build
!/conf
```

This way, only what's needed is sent as part of the build context and it's very
very fast!

## What about the long `Dockerfile` ?

Now, something to note is - we didn't get rid of the long `Dockerfile` - we
still kept it. This is because of the same reason I mentioned before - the long
`Dockerfile` with all the instructions on how to build the code and run it - is still
useful. People with nothing installed on their machine, except Docker, can
build your code's Docker image and run it. If that's something you want, then
you should keep it. Also, it's also kind of like documentation on how to run
your code - each instruction in your `Dockerfile` shows how to run your code in
a production mode. It will be a living documentation if you update it and
maintain it.

You might wonder - then what about the small and fast `Dockerfile` ? Well, we
named it `Dockerfile.ci` as it will be mainly used in the CI/CD pipeline. But,
still, why? Why not just use one `Dockerfile` - get rid of the long one, and
just have the small one. Well, if you notice, anyone who uses this
`Dockerfile.ci` cannot simply run this command directly

```bash
$ docker build -f Dockefile.ci -t local-image .
```

Instead, because of the need/pre-requisite of the `Dockerfile.ci` - which is a
`build` directory with proper `build` content for it to work properly at
runtime, the user has to first run `yarn install` and `yarn build` and then only
can run `docker build`. Ideally people would expect to easily run the Docker
image build with just the standard command and nothing else, which is

```bash
$ docker build -t local-image .
```

And that command uses `Dockerfile` by default, as the file to read. So, for a
good user experience, we just named things appropriately. One thing to note
is - for that good user experience, we also need to maintain and update our
`Dockerfile`. I think documentation and good developer experience is important.

Considering we planned to support both `Dockerfile` and `Dockerfile.ci` - we had
to do something extra in case we wanted to squeeze the performance in terms of
build context content and size that we just saw. For `Dockerfile` - we need all
the code, metadata. For `Dockerfile.ci`, we just need two directories. This
basically means having two separate Docker ignore files for each of these.
Initially on my research, I thought this wasn't possible and only one
`.dockerignore` was possible. But apparently it's possible, check this
stackoverflow answer

[Answer to: How to specify different .dockerignore files for different builds in the same project?](https://stackoverflow.com/a/57774684/4772008)

So, what we did was - use separate Docker ignore files -
* `.dockerignore` for `Dockerfile`
* `Dockerfile.ci.dockerignore` for `Dockerfile.ci`

And while running Docker image build, we used `DOCKER_BUILDKIT=` environment
variable configuration, to enable the feature to use multiple Docker ignore files.

## Gotchas on Strategy

Now, another thing to note is - the environment in which you run the stages of
your pipeline.

If you compare the two `Dockerfile`s

```Dockerfile
FROM node:12.12.0-alpine AS build-stage

WORKDIR /app

COPY package.json ./
COPY yarn.lock ./
RUN yarn install

COPY src ./src

RUN yarn build

FROM nginx:1.19-alpine
COPY --from=build-stage /app/build/ /usr/share/nginx/html
COPY conf/nginx-default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

```Dockerfile
FROM nginx:1.19-alpine
COPY ./build/ /usr/share/nginx/html
COPY conf/nginx-default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

In the first one, the `yarn build` runs in [alpine](https://alpinelinux.org/) linux OS,
with `node` version `12.12.0` installed and it's running in amd64 architecture.
But in the second one, the `yarn build` ran in a previous stage in the Jenkins
Build Stage's environment - which is what? We'll come to that. Ideally, things
are easier if the two environments - Jenkins stage environments helping with
the build artifact and the `Dockerfile` environment - are equal. That is, if you
are going to try the approach I just mentioned - to make your Docker image build
faster. 

But why? Well, it's very possible that the build artifact produced in your
Jenkins stage is not compatible with the environment of your `Dockerfile`, which
is very application specific. So only you can tell if it's something for you to
lookout for or not.

In our case - it doesn't really matter much - as no matter what the environment,
the dependencies are installed and build is run - but the final build artifact
is simply HTML, CSS, Js files, which are not dependent on any environment, and
are simple static files.

And if you miss to notice the difference in environment and that causes some
issue, I'm guessing it will be pretty clear when the issue bubbles up, or else
it's going to be a pain to find it 😅 

In our case, we run our Jenkins pipeline stages inside Docker containers. A lot
of CI/CD systems support using Docker containers as execution environments to
run CI/CD stages or build scripts.

So what we did was, we used `node:12.12.0-alpine` Docker image based Docker
containers as execution environments for anything related to our React App build
stages. 

## Another example using Java

I can give another example where we followed this strategy - this is for Java
application - a Spring Boot microservice.

We run the following stages in our build pipline
* Run Gradle build - which runs compile, test, everything
* Build Docker Image
* Push Docker Image to a Docker Image registry

For this, again we use the build artifact - the jar file of our microservice, in
the `Dockerfile.ci`. It looks kind of like this

```Dockerfile
FROM adoptopenjdk:14-jre-hotspot
COPY ./build/libs/app.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]

EXPOSE 8080
```

So, we are not redoing the `./gradlew build` or any other `gradle` task to
compile or anything. All that is done before hand in our CI/CD pipeline in our
case and we just copy the build artifact into our `Dockerfile.ci`. So, it's
very fast and a very simple `Dockerfile`

As mentioned before, we made sure that the build stage environment is the same
as the environment present in our `Dockerfile` - always using Java 14, linux OS,
amd64 architecture.

## Side note on Docker for development

Some people also create `Dockerfile` or `docker-compose.yml` for development
purposes too. In case you are interested in that area, something for you to
checkout. In those cases, people don't need to install anything in their local
machine - everything is installed in the Docker container and the source code is
mounted into the container and everything runs there. Also, many tools are
playing around with this idea and helping with this. I keep hearing a lot about
the
[Remote - Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
in VS Code Editor. You could check it out! :) I'm yet to try it out.

## Conclusion

It's good to keep the Docker image build fast in your pipeline, and your
pipeline too, in general.

Personally, it felt good for me to remove code in the `Dockerfile` and also
speed things up. And also keep things simple.

Sometimes we might be overcomplicating things when it comes to new technologies.
In this case, it's Docker, and it's usage. I think it's good to do your research
and keep things simple. 

By the way, there are also many tools in the Docker ecosystem.  For example, I
have seen so many tools (surely great ones!) to help with building Docker images -
* [buildpacks](https://buildpacks.io) with support for different languages
* Spring Boot v2.3.2 docs for building Docker image with
[gradle](https://docs.spring.io/spring-boot/docs/2.3.2.RELEASE/gradle-plugin/reference/html/#build-image)
or [maven](https://docs.spring.io/spring-boot/docs/2.3.2.RELEASE/maven-plugin/reference/html/#build-image). It internally uses [buildpacks](https://buildpacks.io/) and [paketo](https://paketo.io/)
* [jib](https://github.com/GoogleContainerTools/jib) - Building container images
for Java applications
* [packer](https://www.packer.io/) - supports Docker images and also Virtual
Machine images, and has support for many cloud platform related services too

There are probably more! These were just a few that I noticed when I was
checking out about building images for our Spring Boot Java application. The
thing is, it's good to research and understand the benefits of any tool that
you plan to use and for what use case the tool was built for. Then check what's
your use case and find if the tool fits your use case. In my case, even though
I tried all the tools and some of them were even easy to use, I felt it wasn't
necessary. So we just went ahead and wrote our own simple, small 
`Dockerfile` manually for our Java application and for our React application
too. It works pretty fine for us!

Let me know what you feel in the comments! You can also ask any questions you
have in the comments! :)
