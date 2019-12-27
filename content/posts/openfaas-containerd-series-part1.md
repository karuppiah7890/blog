---
title: "Openfaas Containerd Series Part 1"
date: 2019-12-27T19:49:59+05:30
---

This is one of the posts in a series where I'm going to write about
[openfaas](https://www.openfaas.com/) and containerd working together.
If you haven't tried openfaas before, it's interesting, give it a try!
This is one of the posts which you can use to try openfaas!

In this first post, I'll just show you what I tried out recently. This
is me showing you how to just run openfaas along with containerd in one
VM. I'm not going to explain the details about how it works and sorts.
I haven't digged deep into the details. I usually like to see something
work before getting into the details of how it works. Without much ado,
I'll start!

First, we need a Linux VM. I chose to run an Ubuntu VM using
[multipass](https://github.com/canonical/multipass/). You can use any
Linux VM. 

I use MacOS, so I installed `multipass` with `brew`. Other OS users follow
[this](https://github.com/canonical/multipass#install-multipass)

```
$ brew cask install multipass
```

Next spin up a VM named `faas` using `multipass`

```
$ multipass launch -n faas
```

Wait for it to start. The logs are clear and tell what's going on and
if the VM has started.

Now ssh into the VM

```
$ multipass exec faas bash
```

Once inside the VM, install `containerd`

```
$ export VER=1.3.2
$ curl -sLSf https://github.com/containerd/containerd/releases/download/v$VER/containerd-$VER.linux-amd64.tar.gz > /tmp/containerd.tar.gz \
  && sudo tar -xvf /tmp/containerd.tar.gz -C /usr/local/bin/ --strip-components=1
```

Check if it works

```
$ containerd --version
containerd github.com/containerd/containerd v1.3.2 ff48f57fc83a8c44cf4ad5d672424a98ba37ded6
```

Now enable portfowarding and made the setting permanent

```
$ sudo /sbin/sysctl -w net.ipv4.conf.all.forwarding=1
$ echo "net.ipv4.conf.all.forwarding=1" | sudo tee -a /etc/sysctl.conf
```

Next install `runc` and check the installation

```
$ sudo apt update && \
  sudo apt install -qy runc \
$ runc --version
runc version spec: 1.0.1-dev
```

Next install `netns`

```
$ sudo curl -fSLs "https://github.com/genuinetools/netns/releases/download/v0.5.3/netns-linux-amd64" \
   -o "/usr/local/bin/netns" \
   && sudo chmod a+x "/usr/local/bin/netns"
```

Check if it works

```
$ netns version
netns:
 version     : v0.5.3
 git hash    : 9b103a1
 go version  : go1.10.7
 go compiler : gc
 platform    : linux/amd64
```

And then install `faas-containerd`

```
$ sudo curl -fSLs "https://github.com/alexellis/faas-containerd/releases/download/0.3.0/faas-containerd" \
   -o "/usr/local/bin/faas-containerd" \
   && sudo chmod a+x "/usr/local/bin/faas-containerd"
```

Next put some configuration in some files

```
$ sudo mkdir -p /etc/cni/net.d
$ sudo vi /etc/cni/net.d/10-mynet.conf
```

Put the below content and write it into the file

```
{
    "cniVersion": "0.3.0",
    "name": "mynet",
    "type": "bridge",
    "bridge": "cni0",
    "isGateway": true,
    "ipMasq": true,
    "ipam": {
        "type": "host-local",
        "subnet": "10.244.1.0/24",
        "routes": [
            { "dst": "0.0.0.0/0"  }
        ]
    }
}
```

Now run `containerd`

```
$ sudo containerd
```

Let `containerd` run in this shell. Now open another shell, ssh into
the VM and run `faas-containerd`

```
$ multipass exec faas bash
$ sudo faas-containerd
```

Again open another shell 😅 and ssh into the VM. This time we are going
to deploy functions. The functions will run in containers. We will use
`faas` CLI to deploy the functions. Get the `faas` CLI first

```
$ multipass exec faas bash
$ curl -sLfS https://cli.openfaas.com | sudo sh
```

Next let's deploy our function! 😁

```
$ faas store deploy figlet -g 127.0.0.1:8081 --update=true --replace=false
```

You will notice the output like this

```
WARNING! Communication is not secure, please consider using HTTPS. Letsencrypt.org offers free SSL/TLS certificates.
Function figlet already exists, attempting rolling-update.

Deployed. 200 OK.
URL: http://127.0.0.1:8081/function/figlet
```

Now check the `containerd` logs and `faas-containerd` logs to see some
action behind the scenes and to know that there are no errors and that
everything is working fine and the deployment went through. Now that
out function is deployed, let's see if it's running and what's the
output it gives

Invoke it using this

```
$ echo "openfaas" | faas invoke figlet -g 127.0.0.1:8081
                         __
  ___  _ __   ___ _ __  / _| __ _  __ _ ___
 / _ \| '_ \ / _ \ '_ \| |_ / _` |/ _` / __|
| (_) | |_) |  __/ | | |  _| (_| | (_| \__ \
 \___/| .__/ \___|_| |_|_|  \__,_|\__,_|___/
      |_|
```

You can see how the "openfaas" string comes as a cool art!
You can try out more strings other than "openfaas" as input 😄

To list all your functions, use

```
$ faas list -g 127.0.0.1:8081

Function                        Invocations     Rep
figlet                          0               0
```

Let's deploy more functions! 😉

Now a function that pings using `ping` command

```
$ faas deploy --image alexellis2/ping:0.1 \
  -g 127.0.0.1:8081 --update=true --replace=false --name ping
```

Let's invoke the function

```
$ echo "-c 1 8.8.8.8" | faas invoke ping -g 127.0.0.1:8081

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=52 time=5.10 ms

--- 8.8.8.8 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 5.095/5.095/5.095/0.000 ms
```

You can see how the ping function gives output! We are basically
running the `ping` command as a function, and the input here is
`-c 1 8.8.8.8`. So, it's the same as running the below command

```
$ ping -c 1 8.8.8.8

PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=53 time=30.8 ms

--- 8.8.8.8 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 30.842/30.842/30.842/0.000 ms
```

More functions? 😉Let's deploy a `nodeinfo` function which gives node
information

```
$ faas store deploy nodeinfo \
  -g 127.0.0.1:8081 --update=true --replace=false
```

Let's invoke it!

```
$ echo "" | faas-cli invoke nodeinfo -g 127.0.0.1:8081

Hostname: localhost

Platform: linux
Arch: x64
CPU count: 1
Uptime: 4105
```

You can see some information about the VM. For verbose information like
the model of CPUs and the networks interfaces, use `verbose` as input
like this

```
Hostname: localhost

Platform: linux
Arch: x64
CPU count: 1
Uptime: 4158
[ { model: 'Intel(R) Core(TM) i7-4770HQ CPU @ 2.20GHz',
    speed: 2193,
    times:
     { user: 230300, nice: 15800, sys: 127800, idle: 40662100, irq: 0 } } ]
{ lo:
   [ { address: '127.0.0.1',
       netmask: '255.0.0.0',
       family: 'IPv4',
       mac: '00:00:00:00:00:00',
       internal: true,
       cidr: '127.0.0.1/8' },
     { address: '::1',
       netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
       family: 'IPv6',
       mac: '00:00:00:00:00:00',
       scopeid: 0,
       internal: true,
       cidr: '::1/128' } ],
  eth0:
   [ { address: '172.19.0.5',
       netmask: '255.255.0.0',
       family: 'IPv4',
       mac: 'c6:43:d5:16:c3:9a',
       internal: false,
       cidr: '172.19.0.5/16' },
     { address: 'fe80::c443:d5ff:fe16:c39a',
       netmask: 'ffff:ffff:ffff:ffff::',
       family: 'IPv6',
       mac: 'c6:43:d5:16:c3:9a',
       scopeid: 10,
       internal: false,
       cidr: 'fe80::c443:d5ff:fe16:c39a/64' } ] }
```

Now that we have many functions, let's see how it runs as containers

So, we have three functions

```
$ faas list -g 127.0.0.1:8081

Function                        Invocations     Replicas
figlet                          0               0
ping                            0               0
nodeinfo                        0               0
```

We can use the `ctr` command to see the running containers

```
$ sudo ctr -n openfaas-fn containers list

CONTAINER    IMAGE                                  RUNTIME
figlet       docker.io/functions/figlet:0.13.0      io.containerd.runc.v2
nodeinfo     docker.io/functions/nodeinfo:latest    io.containerd.runc.v2
ping         docker.io/alexellis2/ping:0.1          io.containerd.runc.v2
```

So, those are your containers. That's it for this post. I have shown
you how to run the cool openfaas in a VM.

You can check more about openfaas and
containerd [here](https://blog.alexellis.io/faas-containerd-serverless-without-kubernetes/)
and about why it's fascinating, even though there are other ways to run
openfaas, like running it on Kubernetes.

Lookout for more posts about openfaas while I try it out! 😁