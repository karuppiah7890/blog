---
title: "kubectl commands"
date: 2020-03-09T09:30:21+05:30
---

Some of the commands that I use in my day to day work when interacting with kubernetes clusters

## Exec / execute into a pod

If you have [kube-fzf](https://github.com/thecasualcoder/kube-fzf), you can use `execpod` which
is an interactive way to exec into a pod's container

```
$ # exec into a pod in the default namespace
$ execpod
$ # exec into a pod in a particular namespace
$ execpod -n <namespace-name>
$ # exec into a pod in any namespace by seeing all namespace pods
$ execpod -a
```

Or if you don't have [kube-fzf](https://github.com/thecasualcoder/kube-fzf), you can use the below

```
$ kubectl exec --namespace <namespace-name> <pod-name> -c <container-name> -it <command>
$ # an example for sh command is below
$ kubectl exec --namespace default ubuntu-pod -c ubuntu -it sh
```

I'll keep updating this when I get time and when I recall the different commands that I use :)

