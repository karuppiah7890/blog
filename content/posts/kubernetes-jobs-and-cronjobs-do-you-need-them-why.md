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

Any thoughts on why someone would have such a requirement?

I can think of some use cases for such a requirement.

- Any sort of one off tasks to run
- CI/CD pipelines - everything there is a task - Compile, Build, Test, Deploy
- Data pipelines which need to run some data processing jobs. Personally I have
  not done this, but I have heard of it.
- Running database migrations. Before you deploy some of your application,
  sometimes you need to run migrations. I know that some people run it as part
  of the application startup. The thing to note is, when multiple instances of
  the application is present, it doesn't make sense to run the migration as
  part of the application startup, though it's possible with locking mechanisms.
  This is just an alternative - run the migration task separately, then deploy
  the application.

Another related requirement could be - run a task again and again. Use case?
Some of the ones that I have personally seen and worked with is - backup of
data based on an interval - every day, every month etc.

< those are just a few examples >

< How does one go about implementing a solution for this requirement? >

< One does Not need Kubernetes to implement this solution or the solution to
any requirement. So, let's say you don't have Kubernetes, what would one use?
Plain servers to run tasks. I have heard some people talk about AWS Lambda
functions which are serverless functions - and there are many implementations
for serverless functions>

<If I want to implement the solution in Kubernetes, how do I go about doing
that? Let's assume that we only have the basic resources - Pods, ReplicaSets
and Deployments>

<Pods can run one-off tasks very easily.>

<a yaml example with restartPolicy never>

<talk about the reason for restartPolicy never and the loop>

<ask if the solution is good enough. talk about how does one check if the pod
task ran successfully or failed. what happens during success and failure?>

<ask what people would do if their job which is probably mandatory failed.>

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
