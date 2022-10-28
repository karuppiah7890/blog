---
title: "Building nginx from source and installing it"
date: 2022-10-28T21:52:30+05:30
---

Today I was trying out the latest version of `nginx`. I was trying to build it from scratch on my MacOS machine, and it seemed pretty straight forward to build and use :D Let's see what I did

> Note: I assume that you have development tools like `make` installed. I installed them by doing `xcode-select --install`

For detailed docs on configuration for compilation, check https://nginx.org/en/docs/configure.html

The latest version of `nginx` as of this writing is `1.23.2` . Find the latest on https://nginx.org/download/ webpage

```bash
wget https://nginx.org/download/nginx-1.23.2.tar.gz

tar xvzf nginx-1.23.2.tar.gz

cd nginx-1.23.2

./configure

make

sudo make install

ls /usr/local/nginx

ls /usr/local/nginx/sbin

echo $PATH

export PATH="/usr/local/nginx:$PATH"

sudo nginx -h

sudo nginx

ps aux | grep nginx

curl http://localhost

sudo nginx -s quit

ps aux | grep nginx
```

Given below is the demo of how I do the above given steps and what output / logs show up when running the commands

{{<asciinema 533008>}}
