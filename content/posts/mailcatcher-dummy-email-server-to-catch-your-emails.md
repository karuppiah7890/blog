---
title: "Bytes #1: Mail Catcher: Dummy Email Server to catch your emails!"
date: 2021-03-27T21:47:35+05:30
---

[MailCatcher](https://mailcatcher.me/) is a nice little tool to help you run a
dummy email server

It's open source - https://github.com/sj26/mailcatcher

Why would one use it? There are two use cases for which I personally used it
for.

One is - trying out a program in local which needs an email server as part of 
setup. I couldn't setup an email server like the ones people do in production 
environments or use email services like AWS Simple Email Service, as I was only
simply trying out the program and not doing a production deployment. So, I used
mailcatcher in this case. The program I was trying out was [Discourse](https://www.discourse.org/)

Another is, I had to develop a solution in our project to send email alerts when
some issue occurs. To test it out, I use mailcatcher dummy email server to see
if mail sending feature works

In both cases I needed SMTP email server to send emails and mailcatcher provided
it.

To check the emails sent to the mailcatcher server, there's a web UI! :D I
always used the web UI. Apparently there's also a HTTP API, but I haven't tried
it out

How to run it? It's written in Ruby. You can use `gem` and install it and run
it. Or you can use Docker, there's an official Docker image for it

Docker Image in Docker Hub - https://hub.docker.com/r/sj26/mailcatcher

Source Dockerfile for the Docker Image - https://github.com/sj26/mailcatcher/blob/master/Dockerfile

I think the below is enough

```bash
$ docker run -d --name mailcatcher -p 1025:1025 -p 1080:1080 sj26/mailcatcher
```

It will run the Docker container in the background with SMTP port and web server
port exposed, the web server serves the web UI

What's my recommendation on the method to run? Use Docker! :) As Ruby
installation, Ruby versions, gem versions, access mode of the directories etc
is too much work. Unless you work with Ruby and are pretty comfortable, you can
go with Docker :)
