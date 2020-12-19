---
title: "Kubernetes DaemonSets: Do you need them? Why?"
date: 2020-12-14T22:53:35+05:30
---

> Note: This article assumes you know about Kubernetes basics like Pods and that
> Pods run on nodes, and that Kubernetes works in a cluster architecture with
> usually more than one node.

## TLDR;

DaemonSet is a specific name of a resource in Kubernetes in case you haven't
heard of it. I like to call it as "run everywhere thing" when I introduce it to
newbies. If you need to run a program / software in every node of the
Kubernetes cluster, then this article might be of interest to you. You can
probably solve this problem in multiple ways using Pods and maybe even using
ReplicaSets and Deployments in case you need some more features that those
provide. When you try to solve the problem using these basic resources you will
end up doing a lot of things, which may or may not look easy for you. But to
make your life easier, compared to whatever solution you came up with, I think
DaemonSet is provided to you as a built-in resource in Kubernetes for solving
exactly this problem / need.

## Longer Version

Recently I was prepping for a Kubernetes internal workshop in my company. Apart
from the usual what and how, we usually give the why behind everything that
we teach during these workshops.

When I was prepping for the content I was gonna talk about, I felt that I could
take it up a notch when it comes to the why part as I felt one could dig in more
and show why DaemonSet exists.

So I started going into the standard questioning mode and starting asking
questions. These are some standard questions that people in tech ask whenever
they see some new tech.

"Why does this thing exist? What problem does it solve? Are there other
alternatives to solve this? Are there simpler ways to solve this? How is this
better than the others? How is it worse than the others?"

Those are simple elaborate questions for the short questions "Why. Why?! Pros?
Cons? Simpler or Complex?"

I learned from some of my colleagues that it's good to show the why first when
talking about some new thing and then teaching about it. There are different
opinions about this kind of teaching. My opinion is that - it usually takes more
time to teach the person.

It's possible that I'm trying to make the person go through the same journey as
me while I'm teaching them. Is that really needed? Can they learn faster and
also not have the hurdles that I had? Maybe, maybe not. It really depends. So I
don't know honestly which is the best in different situations. But I do think
that people remember better when they have seen and experienced worse
alternatives and like the simpler better alternatives. They learn better that
way and also have a good reasoning and experience of why something is better
and why something else is worse.

Just saying something else is a bad alternative sometimes may not resonate well
with the other person. They might be like "oh, okay" and may shrug it off. If
they want to learn, practice and sometimes only failures and experience can
teach some people some things. But it doesn't have to be the same way for all.
Not everyone learns the same way 😅 But I learn this way usually and I wanted
to show some of the alternative solutions to whatever I was going to teach
about and discuss about them and take it from there.

I started off my talk with some abstract terms. I'm going to do the same here
and then we will slowly move forward from there.

Now, let's say we have a requirement / need, or an action to do. It is

"I want to run a software in every node of the Kubernetes cluster or some
special set of nodes in the Kubernetes cluster if not all"

Now, if you don't have such a requirement, or if think you will never have such
a requirement or are not interested in this requirement, I'm not sure if you
would want to read the whole article. So, if this is not a requirement you want
implement / solve, you don't have a problem. If you don't have a problem, you
don't need to think about solutions to it. In which case, you can go ahead and
checkout other articles I have written ;)

On the other hand, if you are interested in solving this problem, let's move on.
So, this is the first thing that comes in your why - Why do you have a solution?
It's because we have a problem/requirement. If you don't have a problem, you
don't need any solution. :)

So, now that we have a requirement, any idea why someone would have such a
requirement? Do you have it? What are you trying to do by doing this? Why would
someone want to run a software in every node of the Kubernetes cluster? We will
get back to the special set of nodes requirement later.

If you think about it, what's the benefit of running the same program in all the
nodes of the cluster? You get access to all the nodes in the cluster. What's the
benefit of that?

Maybe you can run some node specific tasks / work on all the nodes using this
program?

Let's try to be more specific now. I have some examples!

- You can collect logs from all the containers of the cluster. You could maybe
  send it to some remote log storage server for further processing. How? Well,
  all the containers run on the nodes. Nodes have access to all the information
  of the containers including logs. Your program can run in each node and get
  access to these logs and ship the logs to a remote place :) Few examples are -
  [Fluentd](https://www.fluentd.org), [Logstash](https://www.elastic.co/logstash)
- You can monitor your nodes - check node CPU, memory, disk and other node data.
  Monitoring is key in any system. Since all your workloads run in containers
  which in turn is just running on a node - you need to ensure that all your
  nodes are healthy and good and have no issues. For this, you would have to
  check the Node's health time and time. There are many monitoring tools to help
  with such requirements. One example is [Prometheus](https://prometheus.io/).
  You can run Prometheus node exporter in each node and export node metrics to
  monitor the nodes. If it's any other monitoring system, there will be some
  sort of monitoring agent installed in each node which will inform some remote
  server about the node's health. :) One examples is -
  [Prometheus Node Exporter](https://github.com/prometheus/node_exporter/)

There are probably many other use cases for such a thing. These are just the
ones that I have seen and worked with personally.

Now that we know it's a real requirement and not some imaginary need like
many things in the world today 😅, at least that's what I feel 🙈, now, how
would one go about and implement a solution for this requirement?

Let's see how we can solve this with what we already know, alright?

I'm assuming you know some Kubernetes basics, which means you would know about
Pods. Can we implement a solution with Pods? Also, you could also try using
ReplicaSets or Deployments, if you know those and want to use the features of
those.

Now, let's say I use Pods. I could go ahead and run N number of Pods where N is
the number of nodes. Let's say number of nodes is 3. Now, it would look
something like this

![scenario-1](/blog/img/kubernetes-daemonset-do-you-need-them-why/scenario-1.png "scenario-1")

Now, would it always look like this? It might not. This is just one possible
scenario. Why? Well, one of the main features of Kubernetes is it's scheduling
where it schedules / assigns workloads to different nodes based on the
resources available in the node and also by understanding what resources the
workload needs. For example CPU, RAM needed by the workload (Pod) and the
available CPU, RAM in the node. There are also many other factors which can
control scheduling depending upon how you configure your Pods and your Nodes.

So, the above is just an ideal scenario - it's ideal because it helps with
implementing the solution to a requirement we have. There are many other
possible scenarios which are not ideal. Below are two examples

![scenario-2](/blog/img/kubernetes-daemonset-do-you-need-them-why/scenario-2.png "scenario-2")
Scenario 2. Not Ideal Scenario.

![scenario-3](/blog/img/kubernetes-daemonset-do-you-need-them-why/scenario-3.png "scenario-3")
Scenario 3. Not Ideal Scenario.

So, the Pods could be running in any of the nodes due to various reasons. So,
it's very possible that I'm not running my program (within the Pod) in all the
nodes of the cluster.

Now if you have worked with Kubernetes and scheduling Pods a lot, you would also
know that it's possible to control the Kubernetes scheduling using some
configurations.

For example, we can use any of these configuration in the Pod spec

- nodeName
- nodeSelector
- affinity - podAffinity
- affinity - podAntiAffinity

And maybe more. These are just a few examples. Now, I'm going to use one of
these to write a Pod yaml to run a program in all the nodes of the cluster

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-nginx-everywhere
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-nginx-everywhere
      objectType: deployment
  template:
    metadata:
      name: nginx
      labels:
        app: nginx-nginx-everywhere
        objectType: deployment
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchLabels:
                  app: nginx-nginx-everywhere
                  objectType: deployment
              topologyKey: kubernetes.io/hostname
      containers:
        - name: nginx
          image: nginx:1.19-alpine
```

![nginx-nginx-everywhere](/blog/img/kubernetes-daemonset-do-you-need-them-why/nginx-nginx-everywhere.jpg "nginx-nginx-everywhere")

I have tried to deploy an Nginx server everywhere - in all the nodes of the
cluster. I chose Nginx as it's a simple example.

In the above yaml, I have created a `Deployment` which has
`affinity.podAntiAffinity` configuration in the `Pod` spec to inform
Kubernetes that I don't want to run a `Pod` in a node where there's already
a `Pod` in the node with the same labels as this `Pod` - same workload, but
different instance. So, no two instances of the same program can run on the
same node, and if the number of replicas (instances) is equal to the number of
nodes in the cluster, then you have what you need - your program running in
all the nodes of the cluster.

Also, something to note is - all nodes must have enough resources to run your
Pods in each of the nodes, this is a basic necessity irrespective of what kind
of solution or implementation we have, not just this solution. This is based on
the basic requirement that you need resources to run your programs.

With the above yaml or similar configuration which can control scheduling
of pods to nodes, you can easily deploy pods in all the nodes of the cluster.
Now, did we miss something? Are there any problems with such a solution?

What happens when a new node is added to the cluster? Or a node is removed from
the cluster? If a new node is added, you need to increase the replica count of
the `Deployment` or do whatever you gotta do to increase replicas, in case you
used `ReplicaSet` or just plain multiple `Pod`s. And the same goes for removal
of node - you don't want a `Pod` to just be lying there in some Pending state
and it also shouldn't go into a node where the program is already running as
that's unnecessary.

You can manually change the replica count or you could automate it. There are
multiple ways to automate it. You could use the [Horizontal Pod Autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) and
automatically scale up and down based on the metric "node count". If that does
not seem viable, you could write your own custom code which interact with the
Kubernetes API and watches for changes in node count and increase or decrease
the replica count.

So, I guess we have a solution now? Something that can fully automated. :)

Also, in between we spoke about "run program on all nodes or a subset of nodes".
For subset of nodes, you can again control pod scheduling using node related
configuration in Pod spec and ensure it is deployed only in some set of nodes,
for example any node with a particular label. You can also do the same for your
auto scaling - check for the node count only for this special subset of nodes
and then scale your workloads. You are good to go then.

Now, was that a convincing solution? I think it was. Did you feel it was too
much (complex/hard)? Maybe, maybe not. Only you can say.

Let's say you use some other shiny new container orchestration platform that
comes out tomorrow, or even a new scheduler. Let's say you have such a
requirement to run a program in every node or a subset of nodes. Let's say the
tool or platform does not have the feature out of the box as an abstraction.
You would probably have to think about solving it in some similar way like we
discussed above. You would do it mostly because the platform is not able to
help you and ease the process of solving it.

In case of Kubernetes, it provides you with a feature to ease the burden of
going through all the trouble we just went through. I mention it as trouble as
the Kubernetes feature provides a comparatively simpler alternative. The
solution to this requirement is called DaemonSet, as you might have already
guessed. You wouldn't have guessed it if you were in my workshop :P In my
workshop I didn't use the word DaemonSet until it was time to introduce it ;)

So, what is DaemonSet? It's a Kubernetes resource to help you run programs in
all the nodes of the cluster or a subset of nodes. The DaemonSet yaml looks
something like this

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.19-alpine
```

Now, compare this DaemonSet yaml with the Deployment yaml above and apart from
the Deployment, you would have needed some extra yamls to deploy stuff for the
autoscaling feature and maybe even write some code if Horizontal Pod Autoscaler
doesn't help.

I think DaemonSet looks way simpler. That's all you gotta do - the simple yaml
above. That's it.

It will work when new nodes come in, or when nodes are removed from the cluster.
Also, you can notice that there is not replica count config in DaemonSet - can
you guess why?

Yes, it's because the replica count is not a static value - it's based on the
node count and DaemonSet implementation behind the scenes already knows how to
scale the Pods up and down based on node count, so you don't really need to tell
it about any sort of replica count. It knows what it's doing ;) :)

DaemonSet can also be used to run a program in a only a subset of nodes in the
cluster. You can do this by configuring the DaemonSet's Pod template spec
with appropriate node selector or node affinity configuration. You can find lot
of examples for node selector and node affinity online. So I'll leave that for
you to explore :)

Below is a small and simple visualization of how the DaemonSet works with the
DaemonSet controller

![daemon-set-controller-creating-pods](/blog/img/kubernetes-daemonset-do-you-need-them-why/daemon-set-controller-creating-pods.png "daemon-set-controller-creating-pods")

DaemonSet Controller is actively looking at DaemonSet objects and creating pods
by interacting with the API server. It creates pods based on node count, and
other config like node selector and node affinity

![daemon-set-controller-with-created-pods](/blog/img/kubernetes-daemonset-do-you-need-them-why/daemon-set-controller-with-created-pods.png "daemon-set-controller-with-created-pods")

In this case, the DaemonSet does not have any special config to run on a subset
of nodes so it's running on all the nodes of the cluster :)

That's how DaemonSet works and tries to make your life easier if you have a
need to run things on all nodes ;) I hope this post useful for you. Do you have
any questions? Comments? Put them all below or just email me at karuppiah7890 at
gmail.com :)
