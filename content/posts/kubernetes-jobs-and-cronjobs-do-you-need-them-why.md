---
title: "Kubernetes Jobs and CronJobs: Do you need them? Why?"
date: 2020-12-21T05:00:35+05:30
draft: true
---

## TDLR;

Following my blog post about [Kubernetes DaemonSets](https://karuppiah7890.github.io/blog/posts/kubernetes-daemonset-do-you-need-them-why/), I'm writing
this post to talk about Kubernetes Jobs and CronJobs :D

In this blog post we will see what's the use case for Kubernetes Jobs and
CronJobs. We try to solve the same use case without these resources, and then
see how it compares to Kubernetes Jobs and CronJobs.

## Longer Version

Let's start with a requirement first

"I want to be able to run a task and know if it was successfully completed or
not"

Here, task is anything that's not a long running / infinitely running process,
for example a backend server is not a task.

Any thoughts on why someone would have such a requirement?

I can think of some use cases for such a requirement.

- Any sort of one off tasks to run
- CI/CD pipelines - everything there is a task - Compile, Build, Test, Deploy
- Data pipelines which need to run some data processing tasks. Personally I have
  not done this, but I have heard of it.
- Running database migrations. Before you deploy some of your application,
  sometimes you need to run migrations. I know that some people run it as part
  of the application startup. The thing to note is, when multiple instances of
  the application is present, it doesn't make sense to run the migration as
  part of the application startup, though it's possible with locking mechanisms.
  This is just an alternative - run the migration task separately, then deploy
  the application.

These are just a few examples. There can be probably more use cases.

Another related requirement could be - run a task again and again. Use case?
Some of the ones that I have personally seen and worked with is - backup of
data based on an interval - every day, every month etc.

Now, how does one go about implementing a solution for this requirement?

If you think about it, one does not really need Kubernetes to implement this
solution. This is true for any solution to any requirement for that matter. So,
let's say you don't have Kubernetes, what would one use? Plain servers to run
tasks. I have also heard some people talk about using AWS Lambda functions,
which are serverless functions. There are also many other implementations
for serverless functions, AWS Lambda is just one example.

But, if I want to implement the solution in Kubernetes, how do I go about doing
that? Let's assume that we know about and only have the basic resources - Pods,
ReplicaSets and Deployments.

Any thoughts?

I'm sure you got this idea by now. Pods can run one-off tasks very easily. Let's
look at an example Pod that runs a sample `echo` task

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-task
  labels:
    app: simple-task
spec:
  containers:
    - name: echo-task
      image: busybox
      command:
        - "echo"
      args:
        - "network-task"
  restartPolicy: Never
```

The above is just a simple Pod yaml. It runs an `echo` command and that's it.
One thing to note is how I have used the `restartPolicy` as `Never`. If I don't
use this value, then the Pod will keep restarting and also go into a
`CrashLoopBackOff` state again and again after showing `Running` and
`Completed`. This is because Kubernetes will think that the container died and
that it has to bring it back up and keep it running. But the truth is that -
our task is just expected to run for sometime and end. It's not some long
running process like a server.

Is this solution good enough? How do we get to know if the task completed
successfully or not? 🤔

Let's check what's the state of the Pod when the task has been completed
successfully. How do you know it was successful? This is just an assumption
based on checking the logs and also the fact that the task is too simple -
echoing a word, and nothing else complicated.

```bash
$ kubectl get pod --watch
NAME    READY   STATUS    RESTARTS   AGE
simple-task   0/1     Pending   0          0s
simple-task   0/1     Pending   0          0s
simple-task   0/1     ContainerCreating   0          0s
simple-task   0/1     Completed           0          7s

$ kubectl logs -f simple-task
network-task
```

We can see that the Pod state is `Completed`. So that's how we know that the Pod
has successfully executed the task.

That was success scenario. What about failure scenarios?

Let's say you used some bad command instead of `echo`, say `ecoh`. Standard
typos 😅

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-task
  labels:
    app: simple-task
spec:
  containers:
    - name: echo-task
      image: busybox
      command:
        - "ecoh"
      args:
        - "network-task"
  restartPolicy: Never
```

In this case, the container will show a state `ContainerCannotRun`.

```bash
$ kubectl get pod --watch
NAME          READY   STATUS    RESTARTS   AGE
simple-task   0/1     Pending   0          0s
simple-task   0/1     Pending   0          0s
simple-task   0/1     ContainerCreating   0          0s
simple-task   0/1     ContainerCannotRun   0          7s
```

The describe also shows the error about the Pod.

```bash
$ kubectl describe pod simple-task
...
...
Events:
  Type     Reason     Age   From               Message
  ----     ------     ----  ----               -------
  Normal   Scheduled  7s    default-scheduler  Successfully assigned default/simple-task to minikube
  Normal   Pulling    6s    kubelet            Pulling image "busybox"
  Normal   Pulled     2s    kubelet            Successfully pulled image "busybox" in 4.942285545s
  Normal   Created    1s    kubelet            Created container echo-task
  Warning  Failed     1s    kubelet            Error: failed to start container "echo-task": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"ecoh\": executable file not found in $PATH": unknown
```

What if there was no typo or code issue but there was still some valid error
due to some reason?

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-task
  labels:
    app: simple-task
spec:
  containers:
    - name: echo-task
      image: busybox
      command:
        - "sh"
      args:
        - "-c"
        - |
          echo "this is cool!"
          echo "oops, now some error is going to occur!"
          exit 1
  restartPolicy: Never
```

Kubernetes would show the Pod with `Error` state

```bash
$ kubectl get pod
NAME          READY   STATUS    RESTARTS   AGE
simple-task   0/1     Error     0          68s

$ kubectl logs -f simple-task
this is cool!
oops, now some error is going to occur!
```

Now that we have seen how we can check if a Pod has successfully executed a task
or not, what do you think about failure scenarios? Success is all good, but
failed task?

Sometimes we have important mandatory tasks that need to be executed
successfully. For example backup tasks. In case of failures, would you just
leave the task? You would retry right? Or do something to run the task
successfully as it's an important task.

How would you ensure that the task runs successfully no matter what? You could
try retry. How? You could manually create more Pods in case the existing Pod
shows failure state.

Is that okay? Creating more Pods manually. Maybe. Maybe not! In production
environments, we should expect failures not as an exception but as a normal and
be prepared for the worst - multiple issues and hence multiple failures. In
which case, doing things manually can never work out. But let's think how you
would do it manually and then think about automation etc.

If a Pod shows error state, then you can delete the Pod and create another Pod.
Benefits? You just keep using the same Pod. It does not seem that complicate.
Problems? But if you want to check on the logs of the error Pod, you will have
to manually store it or have some automation to store and scrape logs. Other
than that, it's okay to delete the Pod.

What else can you do? You could also run separate Pods on the side with a bit
different names and see if they succeed. Benefits? You get to keep the old pods
for inspection and diagnosis. Problems? Too many Pods for the same task,
cluttering the list of Pods in a cluster. That's just more like a readability
thing.

Let's assume you have automated this by using some sort of application which
checks if the task is successful or not, and if it's not successful, then
delete and create Pods or else maybe add more Pods.

Also, what if the task keeps failing always? Due to some other issue that you
haven't found yet. With retry mechanism, especially the automated one, your
tasks would retried again and again about a gazillion times. That's running
the task unnecessarily. So, you need to ensure your automation or manual
creation of Pods to run tasks runs only for a few times and does Not have
infinite retries. This is more like a threshold of when to stop or which nth
time to stop when error has occurred many times. So, more like a max retry.

Now that we have seen about how to run a simple task in Kubernetes using our
most basic Pods resource.

Now, let's say I need to run the tasks multiple times and that too in parallel
to speed things up. I'm not sure what's the use case for this though. I can
only think of one thing based on my experience - deleting a Pod continuously
from inside the cluster. This is just one example.

Now, let's see how to run multiple tasks in parallel.

<talk about how to run multiple jobs in parallel>
<talk about how a example Pod yaml can just be replicated and multiple pods
can be created in parallel to run in parallel. Mention you have not personally
had to do it>
<maybe talk about ReplicaSet and Deployment too, apart from Pods>

<What about running a part (not all) of the tasks in parallel while others
haven't started.>

<What about running the same task again and again and again, but not in parallel
instead in sequence.>

<talk about retry. show how retry can be done with just Pods - but manually>

<Show how kubernetes makes it all easy>

<an example kubernetes Job>

<mention that Job is just an abstraction over Pods with some extra features>

<talk about how to run a task based on an interval or schedule.>

<show yaml example with a plain simple code for interval or schedule>

<now talk about Kubernetes CronJob>

<show an example of Kubernetes CronJob yaml>

<mention that CronJob is just an abstraction over Jobs>

<mention crontab.guru to understand how cron schedule will work. any local tools
reference also will help>
