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
delete and create Pods or else maybe add more Pods. This application would use
the Kubernetes API and do it's work.

Now, what if the task keeps failing always? Due to some other issue that you
haven't found yet. With retry mechanism, especially the automated one, your
tasks would be retried again and again about a gazillion times. That's running
the task unnecessarily too many time. So, you need to ensure your automation or
manual creation of Pods to run tasks runs only for a few times and does Not have
infinite retries. This is more like a threshold of when to stop in terms of time
or which Nth time to stop when error has occurred many times. So, more like a
max retry.

Now we have seen about how to run a simple task in Kubernetes using the most
basic resource - Pod.

Let's say I need to run the same task multiple times - in sequence or in
parallel to speed things up. How would you do that? One example use case for
this is - workers to do some tasks from a task queue. Each worker can do just
a single task, or maybe multiple tasks from the the task queue. It all depends
on the use case.

Now, let's see how to run multiple tasks in sequence and parallel using just
Pods.

Do you already have any ideas?

So, you could create multiple Pods each with a different name one by one in
sequence for sequential run. For parallel run, you can create the Pods
parallely.

For parallel run, you could also use `ReplicaSet` or `Deployment` with multiple
replicas.

Now that we have seen how to do all this using just Pods, ReplicaSets and
Deployments, what do you think about the above solutions?

If you notice closely, a lot of the time we were doing things manually. For
automation, I mentioned creating an application to talk to the Kubernetes API
server and programmatically create Pods or even ReplicaSets or Deployments.
This would mean you have to spend some effort in writing an application for
automation. Or look for an existing application, like an Open Source Software.

Can Kubernetes help here? Of course. This is where Kubernetes Job resource
comes in. You don't need to write any sort of application for automated
creation of Pods to run your tasks. Kubernetes has all features for most of your
usual use cases. It is all possible using the Job Controller present in
Kubernetes.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(20)"]
      restartPolicy: Never
```

Kubernetes Job is just an abstraction over Pods with some extra features. What
are these features? It's all of the above that we just spoke about ;)

With Kubernetes Jobs you can also easily find if the task has been completed or
not.

```bash
$ kubectl get job
NAME   COMPLETIONS   DURATION   AGE
pi     1/1           74s        80s

$ kubectl get pod
NAME       READY   STATUS      RESTARTS   AGE
pi-9xgr4   0/1     Completed   0          75s
```

Look at how the Job shows the number of completions - one out of one (1/1).

How are these Pods created? Job Controller manages and takes care of the Jobs.
It looks at the Job objects and interacts with the Kubernetes API server and
creates Pods and then Kubernetes maintains the Pods. So, if you don't use
Jobs and instead create an application for automating Pod creation for your
running your tasks, it will be basically doing what the Job controller does.
So, why not just use the Job controller? ;) Unless it doesn't satisfy your
use case / needs.

With Kubernetes Jobs you also get automatic retries. So, if a Pod fails with
some non-zero exit status, a new Pod is again created to run the task. Let's
look at an example

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi-retry
spec:
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["bad-command"]
      restartPolicy: Never
```

```bash
$ kubectl get job
NAME              COMPLETIONS   DURATION   AGE
pi-retry   0/1           31s        31s

$ kubectl get pod
NAME                    READY   STATUS               RESTARTS   AGE
pi-retry-92pj4   0/1     ContainerCannotRun   0          113s
pi-retry-d2944   0/1     ContainerCannotRun   0          53s
pi-retry-jj7mk   0/1     ContainerCannotRun   0          2m11s
pi-retry-m6nqg   0/1     ContainerCannotRun   0          2m3s
pi-retry-nltc8   0/1     ContainerCannotRun   0          93s
```

The Pods are created a lot of times to do the retry. Are infinite number of Pods
created? No. Instead there is a max retry count - 6, which is the default. You
could change the default too, using a field in the Job spec called
`backoffLimit` like this

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi-retry-with-2
spec:
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["bad-command"]
      restartPolicy: Never
  backoffLimit: 2
```

```bash
$ kubectl get pod
NAME                    READY   STATUS               RESTARTS   AGE
pi-retry-with-2-chvkr   0/1     ContainerCannotRun   0          2m49s
pi-retry-with-2-slf4w   0/1     ContainerCannotRun   0          2m58s
pi-retry-with-2-zmblx   0/1     ContainerCannotRun   0          2m39s

$ kubectl get job
NAME              COMPLETIONS   DURATION   AGE
pi-retry-with-2   0/1           3m24s      3m24s

$ kubectl describe job pi-retry-with-2
...
...
...
Events:
  Type     Reason                Age    From            Message
  ----     ------                ----   ----            -------
  Normal   SuccessfulCreate      3m53s  job-controller  Created pod: pi-retry-with-2-slf4w
  Normal   SuccessfulCreate      3m44s  job-controller  Created pod: pi-retry-with-2-chvkr
  Normal   SuccessfulCreate      3m34s  job-controller  Created pod: pi-retry-with-2-zmblx
  Warning  BackoffLimitExceeded  3m14s  job-controller  Job has reached the specified backoff limit
```

You can see how the description of the job says that the Job has reached the
specified backoff limit. At times more or less Pods are created. In this case,
3 got created even though backoff limit has been mentioned as 2.

We have seen the basic stuff now. Apart from this, you can also run multiple
tasks in sequence and in parallel using Jobs.

For a simple sequence run, it looks like this

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  completions: 4
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(10)"]
      restartPolicy: Never
  backoffLimit: 4
```

What this means is that - the job has to be run / completed 4 times. This is
done in sequence by default.

For parallel run, you can just do

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  completions: 4
  parallelism: 3
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(10)"]
      restartPolicy: Never
  backoffLimit: 4
```

What this means is - run 3 tasks in parallel at a time, and complete the task
4 times totally. You can also use same value for `completions` and
`parallelism`.

So, that's how you do all the stuff we spoke about but with Kubernetes Jobs with
ease :D

To know more about the different options, you can use the official
[Kubernetes Documentation For Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job) and also use the `kubectl explain` command like this

```bash
$ kubectl explain job
$ kubectl explain job.spec
$ kubectl explain job.spec.completions
$ kubectl explain job.spec.parallelism
$ kubectl explain job.spec.backoffLimit
```

I would also recommend you to explore the extra options like
`activeDeadlineSeconds`, `ttlSecondsAfterFinished` in the Job spec :D

## Kubernetes CronJobs

Kubernetes also has a resource called CronJob which helps you to run tasks like
Jobs. Unlike Jobs, CronJob runs the task again and again though, based on a cron
schedule.

Remember we spoke about backup tasks on a daily or monthly basis? CronJobs can
help with this to run that task, that too in an automated manner.

If you didn't have CronJobs, you would have to manually run Pods based on a cron
schedule or have an application.

<show yaml example with a plain simple code for interval or schedule>

<now talk about Kubernetes CronJob>

<show an example of Kubernetes CronJob yaml>

<mention that CronJob is just an abstraction over Jobs>

<mention crontab.guru to understand how cron schedule will work. any local tools
reference also will help>
