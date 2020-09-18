---
title: "Nginx as Gateway"
date: 2020-09-18T08:46:20+05:30
---

In our project, we are building a web app, with backend. We use a separate
service called [Keycloak](https://www.keycloak.org/) for Identity and Access
Management.

Given we have all this, initially we were exposing these through the same URL
but with different ports
* Port 80 for our web app
* Port 8080 for the backend
* Port 80890 for Keycloak

The user experience for the user accessing the web app looked something like
this - 
* Go to http://web-app-url.com
* Get redirected to http://web-app-url.com:8090/auth/some-more-path (Keycloak)
in case the user is not logged in
* After logging in, they go back to http://web-app-url.com

We wanted the experience to look something like this -
* Go to http://web-app-url.com
* Get redirected to http://web-app-url.com/auth/some-more-path (Keycloak)
in case the user is not logged in
* After logging in, they go back to http://web-app-url.com

Notice the URLs. There is no port in the second one. We wanted it all to look
like it's all coming from the same URL and no different URLs / ports. Even for
the backend, we wanted to expose our backend with URL for example
http://web-app-url.com/api/resource instead of http://web-app-url.com:8080/resource

There can be different expectations too. For example, expose the Keycloak at
http://auth.web-app-url.com and backend at http://api.web-app-url.com . It
really depends upon someone's use case. I don't know much about the differences
between the path based differentiation vs sub domain based differentiation.
Probably a blog post for another day after some research ;)

Now, given we had these expectations, we understood that we need some sort of
gateway to do routing for us, based on the URL / URI path.

`/auth` -> Keycloak

`/api` -> backend

`/` -> web app

Notice that these are prefixes match and not exact paths for the routing we expect,
as Keycloak can have it's own path and it's a webapp with a backend too. For
it to work properly, the path, parameters everything has to be intact for it
to work - fetch html, css, js and other resources and also connect with it's
Keycloak backend.

Given these are prefixes, what this means is that, taking some examples

`/auth/index.html` should go to Keycloak

`/auth/css/index.css` should go to Keycloak

`/auth/js/index.css` should go to Keycloak

`/api/resource` should go to backend

`/index.html` should go to web app

`/css/some.css` should go to web app

`/js/some.js` should go to web app

The web app is kind of like the fallback - when it's **not** `/auth` or `/api` ,
it goes to web app. As `/` prefix match can match all URLs, as all paths start
with `/`. So, it's like a switch case in a programming language - you try to
match the URL path with `/auth` , and then `/api` and then fall back to `/`
to do the routing. You can also switch the checking for `/auth` and `/api`, and
check `/api` first, then `/auth`, it shouldn't be a problem.

Given we have the expectation, we just had to do the implementation. Clearly you
know that the blog post is about [Nginx](https://nginx.org/) based on the title,
so yeah, you guessed it right, we chose Nginx to do our routing.

If you haven't heard of [Nginx](https://nginx.org/), it's an open source web
server, and a lot of other things too. It can be a load balancer, reverse proxy.
I don't know what else, but I do know the software is pretty cool and there are
also commercial versions of it with more features.

You can find the open source code for nginx at these places -
http://hg.nginx.org/nginx/
https://github.com/nginx/nginx (mirror)

There are other web servers that can help with this routing too, like
[Apache httpd](http://httpd.apache.org/) and many others. We just chose Nginx.
I have used it in production before and it has worked out well for us.

Now, coming to the implementation part of our problem, we had to configure our
Nginx server to do the routing based on our expectation. And this kind of
routing is called proxying and our nginx is actually a reverse proxy in this
case. Proxy is used to mask the identity of the client - proxy sends requests
on behalf of the client and the server thinks the proxy is the client. 
Reverse proxy is used to mask the identity of the server / servers, client
thinks all the requests are going to the same server, but it's very possible
that behind the scenes there are many servers / applications or even multiple
instances of the same application, in which case the Nginx server will be a
reverse proxy and a load balancer too, when routing to multiple instances of
same app.

To exactly configure the Nginx, we need to use something called the Nginx
conf - short for Nginx configuration. It's a configuration file, and there are
many ways to configure it and have multiple files too.

There are many blog articles about configuring Nginx, and also ebooks and nginx
docs also help. So, I won't get into too much details of the configuration
itself. In short, what we used is what's called the
[`proxy_pass`](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass)
directive. Directive is an Nginx term for it's configuration.

To run nginx quickly, you can use [Docker](https://www.docker.com/) if you are
familiar with it, or check out different ways to download and run nginx. For
linux, you can find it here
https://www.nginx.com/resources/wiki/start/topics/tutorials/install/ . It
shouldn't be hard to find the same for Windows and Mac. If you use `brew` on
Mac, you can do `brew install nginx`

Nginx looks for it's config files usually in a specific directory, but you can
specify the config file and other path while starting Nginx too.

I'm using Docker to run Nginx like this

```bash
$ # runs nginx in the background
$ docker run -d --name nginx -p 8080:80 nginx:1.19-alpine
$ # to check the logs
$ docker logs -f nginx
```

```bash
$ # getting inside the container, where nginx is running
$ docker exec -it nginx sh
```

```bash
$ # environment where nginx is installed
$ ls /etc/nginx
conf.d          fastcgi_params  koi-win         modules         scgi_params     win-utf
fastcgi.conf    koi-utf         mime.types      nginx.conf      uwsgi_params
$ # the config file is this
$ ls /etc/nginx/nginx.conf
/etc/nginx/nginx.conf
```

We will be putting our sub config files in here - `/etc/nginx/conf.d`. You can
notice in `/etc/nginx/nginx.conf` contents, the line

```
include /etc/nginx/conf.d/*.conf;
```

So, all the config files in `/etc/nginx/conf.d` with extension `.conf` will be
included and processed.

```bash
$ ls /etc/nginx/conf.d/default.conf
/etc/nginx/conf.d/default.conf
```

The default config file looks like this

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

Removing all the comments, looks something like this

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

We are going to change the contents of this file to

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location /api/ {
      proxy_pass http://web-app-url.com:8080/;
    }

    location /auth/ {
      proxy_pass http://web-app-url.com:8090/auth/;
    }

    location / {
      proxy_pass http://web-app-url.com/;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

Note the slashes everywhere, it's important. Or else some weird behaviors will
be noticed. I'll link to articles that can help you understand about it.

Now, the proxying works. Sometimes, it's possible that the apps may behave
weirdly. There could be different reasons for this - I can think of two of them -
if the app depends on the `Host` http header to do some of it's work, or wants
to know the IP address of the client. This is because, when proxying, Nginx
changes the `Host` http header to the value you pass in `proxy_pass`. Coming to
the IP address of client - for the app, it will look like Nginx is always the
client and the IP address of the Nginx is the client IP address. To help with
these two, what you can do is, use the
[`proxy_set_header`](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header)
and set the `Host` header to the Nginx Host name and also set a header called 
`X-Real-IP` to tell the app behind Nginx what's the actual IP of the client. So,
it will look something like this

```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;

    location /api/ {
      proxy_pass http://web-app-url.com:8080/;
    }

    location /auth/ {
      proxy_pass http://web-app-url.com:8090/auth/;
    }

    location / {
      proxy_pass http://web-app-url.com/;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

So that's what we did in our project :) And everything works smooth and the
user experience is neat too!

Some more links for you to read through:

https://dev.to/danielkun/nginx-everything-about-proxypass-2ona
https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
https://nginx.org/en/docs/http/ngx_http_proxy_module.html
https://www.liaohuqiu.net/posts/nginx-proxy-pass/


There are tons of other resources too. The above are just some of the links that
I used

Something to note is that, there are many web servers in the market. Each
offering their own features, some unique, some very usual. I have seen some
use cases where people want to dynamically change the configuration of a web
server or for a reverse proxy/gateway, like in our case, to change it's routing,
and not have to do reload  configuration in the server through SIGHUP signals or
restart the server after updating config. Nginx needs SIGHUP to reload it's
config if the config has been changed. One example of dynamic config through
REST APIs (HTTP API) for gateways is -  [Kong](https://konghq.com/) API Gateway
which is built on top of Nginx. There are more fancy proxies and gateways too,
especially to help with proxying - [Envoy Proxy](https://www.envoyproxy.io/),
[Ambassador Gateway](https://www.getambassador.io/) which is built on top of
Envoy Proxy. 

Choose what suits you the best. I usually think that starting simple could help.
Nginx is just too simple :)

Do let me know your thoughts in comments!
