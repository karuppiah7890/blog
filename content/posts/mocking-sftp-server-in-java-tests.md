---
title: "Mocking SFTP Server in Java Tests"
date: 2020-12-10T19:40:35+05:30
draft: true
---

## TLDR

I wanted to run an actual mock SFTP server for my Java tests. There are different methods. I know two of them -

- Use the Apache SSHD library - [Website](https://mina.apache.org/sshd-projec),
  [Source Code](https://github.com/apache/mina-sshd). With this you can run a
  SSH Server with some lines of Java Code
- Run an external service using something like
  [Testcontainers](https://www.testcontainers.org). Testcontainers library
  helps you run services inside Docker containers. Testcontainers has support
  for Java and many other languages.
  [Source Code for Java](https://github.com/testcontainers/testcontainers-java),
  [Source Code for all languages](https://github.com/testcontainers)

You might not want to run an actual mock SFTP server for various reasons, in
which case, I would recommend checking out a different article and libraries to
mock differently.

External Service - there are many SFTP softwares. I have provided a list below
that I noticed online and/ tried :)

## Longer version

If you are writing some code where you need to integrate with an SFTP server,
and if you need to write tests, you would generally check by having a mock
SFTP server.

What do I mean by integrate? For example, in my case, I had to pull XML files
from an SFTP server whenever new XML files were created in the server. I was
using Spring Integration to integrate with the SFTP server in my Spring Boot
Java application.

It doesn't matter what library or framework you use. As long as it's Java, this
blog should help. Even if it's not Java, I have some recommendations for you :)

Now, let's get started.

---

SFTP Servers I found online

https://github.com/drakkan/sftpgo

https://hub.docker.com/r/amimof/sftp
https://github.com/amimof/sftp

https://hub.docker.com/r/asavartzeth/sftp
https://github.com/AsavarTzeth/docker-sftp/blob/master/README.md

https://hub.docker.com/r/atmoz/sftp
https://github.com/atmoz/sftp

Pointers for testcontainers container images - Run using alpine images maybe?
Unless you are facing issues with alpine, then you can
move on to debian
https://www.turnkeylinux.org/blog/alpine-vs-debian
