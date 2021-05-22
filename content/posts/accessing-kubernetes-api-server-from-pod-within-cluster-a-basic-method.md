---
title: "Accessing Kubernetes API server from pod within cluster: A basic method"
date: 2021-05-22T19:00:10+05:30
draft: true
---

There are probably many blog posts and answers on the Internet around this.

This is the first answer I found when I was looking for this long ago

http://stackoverflow.com/questions/30690186/ddg#30739416

There's also the official docs -
https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/#accessing-the-api-from-a-pod

I recently noticed a similar question coming up on the [Kubernetes slack](https://kubernetes.slack.com) so I wanted to talk about it

The slack thread is here - https://kubernetes.slack.com/archives/C09NXKJKA/p1621604666059400

I'm writing this blog post based on what I had posted in the slack thread

Role based access control - https://medium.com/@antoine_martin/kubernetes-access-the-api-inside-a-pod-eb49af8c8b06

Using `curl`. Show command

Using `kubectl` ? Show command. Also, `kubectl` needs kube config file with - cluster, user and context. Context is basically cluster + user.

References:

https://duckduckgo.com/?t=ffab&q=access+kubernetes+api+server+from+pod&ia=web&iax=qa

https://kubernetes.io/docs/tasks/run-application/access-api-from-pod/#accessing-the-api-from-within-a-pod
