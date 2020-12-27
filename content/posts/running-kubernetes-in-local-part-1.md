---
title: "Running Kubernetes in Local Part 1"
date: 2020-12-27T14:42:35+05:30
draft: true
---

I have been trying to learn Kubernetes a bit in depth. I have used Kubernetes
as a simple user for a long time and I have also digged a bit into the details
of some of the subjects in Kubernetes and also done some basic workshops based
on that knowledge.

I have also tried to do the kubernetes the hard way tutorial, but totally by
blindly following the steps. That too, I was only about to run v1.15.x I think,
and in another similar tutorial, totally scripted tutorial, again I ran v1.18.x.

This time, I didn't want to blindly copy paste commands and not see what's in
them. I did learn a thing or two by doing those tutorials, as some of them were
not exact replica of what I was doing. But of course there was a lot of copy
pasting done blindly.

So, I said to myself "how about I try to run a kubernetes cluster in a top to
bottom approach all by myself?". I have started doing it and I wanted to write
a post about it so here it is! :) By the way, I did end up referring to the
kubernetes the hard way apart from the official kubernetes docs and stack
overflow and other sites for some of the issues I faced. But it was fun to try
out things. Also, I am using the latest stable version of kubernetes that is
available.

Anyways, let's get started and see how to run a kubernetes cluster in local.
Like I said, let's start in a top to bottom approach, which means, you get a
high level idea of what you need and then you get started with that and build on
top of it as more lower level things are needed.

I was reading some of the google search results about Kubernetes and noticed
that the kubernetes components is one of the important ones. It comes right
after the kubernetes overview. This is the same in the official docs and that's
what reflects in the search results.

The components doc (link) talks about the different kubernetes components
involved in a kubernetes cluster.

Let's start with the basic list for our use case

For the control plane components, we will need

api server
etcd
controller manager
scheduler

cloud controller manager is not needed as we don't have any sort of cloud
stuff to do

For worker nodes, we need

kubelet
container runtime
kube-proxy

container networking stuff may also be needed in the worker node

addons - we will come to this later but yeah, we do need basic stuff like DNS.
others, meh, maybe not for our basic local setup to start off with

Now, my wish was to run everything in local, either in my MacOS, or to keep it
clean and safe and easy to cleanup later, I decided to use Virtual Machine (VM).
I decided to use either multipass or vagrant with virtual box and ended up with
multipass :)

Now, for this first part, we are just going to run the API server. What does the
API server need? It needs a store - which is our etcd, so we will run that too!
How do we know the API server works? We can hit the endpoints using a HTTP
client or easier, use kubectl for the most basic things.

So, we will try to run the API Server with etcd and connect to the server using
kubectl. Is it possible? Yes :) Kubernetes being such a distributed platform
with each component so decoupled, you can run them easily in a standalone
fashion to a good extent :) So, you don't really need the other components yet.
This is still a good step :) :D

> multipass launch machine

> multipass exec into machine

> tools to install for help with running stuff

run one etcd server without protection / security

create certificates for some stuff, and also create CA certificate first! :)
cfssl and cfssljson stuff. certificate for CA, api server for HTTPS, service
account for token signing and verification stuff and a user for authentication

run one api server with protection

configure kubectl

use kubectl to access the API server

try to create pod

fix the default service account issue

create pod

notice it's unscheduled because of the scheduler and also the fact that there
is no kubernetes worker node to run the task / pod

We will build things up slowly and steadily :)
