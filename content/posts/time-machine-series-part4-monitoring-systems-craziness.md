---
title: "Time Machine Series Part 4: Monitoring Sytems Craziness"
date: 2021-09-01T22:12:30+05:30
draft: true
---

We were using Prometheus in all our K8s clusters to monitor the cluster

The craziest and probably the funniest thing that happened was - Prometheus was turning out to be pretty resource heavy. We had lots of exporters exposing metrics from different workloads. Prometheus was scraping from all of that. There was alert manager too working with the prometheus. Sometimes we also had Grafana in each cluster which I think we later removed as we wanted a single centralized dashboard and there was already a Grafana hosted in a separate systems (platform team) cluster with other platform team centralized components / services

Since Prometheus seemd to be resource heavy, we had to start putting resource requests and limits and sometimes we noticed it to struggle too. Later we created a separate node pool in the clusters and called it as system components of sorts and put them in separately when we realized that Prometheus was putting strain on the other workloads running on the worker nodes

It's funny and crazy because monitoring system is supposed to help us monitor the systems and help us understand what's the status of the system at a high level and help us worry less, but here we were, in a situation where our monitoring system was putting strain on the system causing us to worry more about the system

A good lesson learned is - it's good to have monitoring, observability, but it's also important to ensure such systems that do monitoring and observability are light weight to the extent that it doesn't strain the system that it's monitoring or observing. Or else people lose trust in the monitoring and observability systems or in the concepts itself in the worst case, though the concepts and ideas are really good and worth it

A good example that I can think of is Redis monitoring that I learned about recently where they clearly call out the cost of enabling latency monitoring in Redis - https://redis.io/topics/latency-monitor#how-to-enable-latency-monitoring where they mention about what happens when we enable the monitoring and how it's probably not worth it if the Redis is known to be doing well

The latency monitoring system within Redis sounds cool too, I have read some things about it. And I think it's awesome that software now a days comes along with monitoring systems around it, unlike some software where community folks build monitoring systems and tools and what not. But yes, most systems have some sort of endpoint / way to provide some basic details at least, if not all internal details for monitoring and observing the system
