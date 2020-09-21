---
title: "Getting Started With Keycloak"
date: 2020-09-21T08:05:21+05:30
---

For me, usually the fastest way to getting started with any big software -
server side software, with other components (backend, proxy etc) is using
[`docker`](https://www.docker.com/) and/
[`docker-compose`](https://docs.docker.com/compose/).

[Keycloak](https://www.keycloak.org/)'s basic setup would be a Keycloak server
with a database. It supports many databases actually.

The quickest way to get started with Keycloak would be to run Keycloak with
[H2](https://h2database.com/html/main.html) database, like this using `docker`

```bash
$ docker run -d --name keycloak -p 8080:8080 -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin jboss/keycloak
```

It will take a few seconds or a minute to bootup. Check the logs using this

```bash
$ docker logs -f keycloak
```

Look for the log saying this

```bash
Admin console listening on http://127.0.0.1:9990
```

Now you should be able to go to your browser and go to the link
http://localhost:8080

It will take you to http://localhost:8080/auth

![keycloak-home](/blog/img/getting-started-with-keycloak/keycloak-home.png "keycloak-home")

Click on the `Administration Console` and type in the Admin username and
password that you passed while running the docker container. I used `admin`
and `admin` as the credentials

![keycloak-admin-login](/blog/img/getting-started-with-keycloak/keycloak-admin-login.png "keycloak-admin-login")

After logging in, you will see the Admin Console like this

![keycloak-admin-console](/blog/img/getting-started-with-keycloak/keycloak-admin-console.png "keycloak-admin-console")

You can look around, discover things, and get curious now :) I'll write more
about using Keycloak in the upcoming articles and maybe even do YouTube videos
to assist the blog content or as video alternatives :)

You can comment on the post with your questions and thoughts :)
