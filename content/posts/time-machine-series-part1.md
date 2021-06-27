---
title: "Time Machine Series: Part 1"
date: 2021-06-27T18:25:20+05:30
---

In this time machine series I want to recall some of my older experiences that I didn't write blog posts about. It's basically me going back in time and recalling some of the cool things that my team did

## Migration of components

In this blog post I'm gonna tell about one of the stories from my work from around 2019 end / 2020 start if I remember correctly

Me and my team had this task of migrating a lot of components - Postgres databases and Redis databases / caches from one Kubernetes cluster to multiple clusters

Why? Our client was having issues with managing team specific services and was also finding it hard to understand the cost of running the team specific services

For example, if there are 3 teams - team A, team B, team C, and each has their own set of services that they maintain, say service A1, service A2, service A3, service B1, service B2, service C1, service C2 etc, when running on Kubernetes - all the services run in a shared infrastructure

We were using Google Cloud Platform (GCP) and in GCP, we were using Google Kubernetes Engine for managed Kubernetes service, instead of managing our own Kubernetes cluster. Surprisingly even GKE was too much to "manage". Anyways, what happened was - GKE was part of a particular GCP project and billing happens at project level from what I know. So, the client management had issues understanding the cost of running each team's services team wise - why? As all services were running in a shared infrastructure - a single Kubernetes cluster, with a few number of worker nodes, that is a single GKE cluster, and hence it was all under a single GCP project, and so there was one bill - which constituted the bill for all team's services

One could do some crazy stuff to find out the cost of each team's services in such a setup, but it can be very tedious. I mean, first, one has to understand the pricing model of GCP and GKE, and then start monitoring the resource usage of the team's services in the Kubernetes cluster and then finally based on resource usage and shared infrastructure - one worker node running multiple services which can / cannot be different team's services, one can find out a billing for each service / each team. Something to note is that - sometimes service ownership can go to different teams, so it's better if the calculation is done per service

Now, why did the client try to do this? Well, I think it's pretty common? I'm not sure though, let's dig in to understand what it was first. So, the client was a product company, they were a Business to Consumer (B2C) company and they were providing services to their customers through mobile apps and lots of people behind it including software engineers. Each of their services was backed by multiple softwares - microservices built by teams, Postgres database, Redis database / cache etc. Since each service used by the customer will have some revenue / profit, it's inevitable that the client will now try to answer questions like -

- How much revenue does this service in our app make for us per month?
- How much money do we spend / invest in this service per month to keep up and running smoothly?
- Based on the answers to the first two questions, does this service lead to profit? Or loss? Are we spending too much on a service that doesn't serve us?

This is a pretty common thing for a business to do. A sustainable business to do. I mean, at times they can take a loss, for the long term profit I guess? I don't know about those stuff, but this is what lead them to think about the cost of the services I think. At least that's what I grasped from the conversations that some of the management folks had

What else was at play? Well, the other thing at play was - helping teams manage their own services. I mean, previously we did this by the popular Kubernetes namespace concept. This time the management decided that apart from the cost finding reason, we can also let teams handle and take care of their own services and components - basically complete ownership for the team for running their services, which was a cool thing

Now, what did this mean for us as an infrastructure / platform team? We had to prepare to enable the product teams for this transition

We had to move the components and services from one Kubernetes cluster to multiple other team specific Kubernetes cluster

Actually, it was - team specific, country specific, staging / production cluster. We had different infrastructures for different countries where the service was available to customers and also had staging and production clusters

What were the components we had to move? Mostly Postgres and Redis (single / ha / cluster)

What tools did we use to migrate the components? For starters, we were already using Helm for managing releases of the components, actually an abstraction over Helm - [Stevedore](https://github.com/gojekfarm/stevedore), which is now open source :D

We built another tool on top of Stevedore called Lofty to get help with migration. The name Lofty is based on the Lofty character from Bob the Builder cartoon :D http://img1.wikia.nocookie.net/__cb20120827142507/btb/images/0/00/Lofty.jpg which I didn't know about at the time 😅 But it was an interesting name and I learned about how one can give interesting names to things :D Apparently Lofty character is good at moving things! ;) And we were doing just that!

Unfortunately Lofty is not open source

So, we wrote the tool to migrate from one Kubernetes cluster to another!

The strategy for the component migration was - replicate from component in one cluster to component in another cluster. Finally, failover to let the destination component take over and be the primary

Putting that in a step by step process -
- Ensure the primary component in the existing cluster is ready for streaming replication by configuring anything that's needed
- Create a new component in the new cluster with a streaming replication of data from the primary
- Ensure component clients can reach the new component after failover - DNS to point to primary component / proxies which in turn point to primary component, no IP caching for components from component client side, so that when there's a failure, DNS resolution happens again
- Finally, when traffic is low, do failover and let the secondary take over as primary
- Later delete the primary once the migration is all good :D

In the case of Postgres, we stopped the clients for a moment and did the failover to avoid any problems / inconsistent data. This was done very momentarily till the failover took place in a few moments and then clients were started back again. In our case, the clients were always microservices

We used [Stolon](https://github.com/sorintlab/stolon) for abstraction over Postgres High Availability (HA) and streaming, for easy automation. But no stolon operator or any operator

Redis - We directly used Redis features. No operators in this case too

Come to think of it, our tech lead used to suggest creating / using an operator for easier management - codifying all the things that we did manually. Pretty cool idea ! :D

So, we did all this using Helm charts. We ensured that we had some knobs (`values.yaml` + features) in the Helm chart to start streaming replication, to do failovers etc with the help of Helm charts and Helm releases and upgrades. We had used hooks and some complex logic at times to make it work

The only tedious thing was that - we had to look at long diffs - differences, when using Stevedore

Stevedore internally used [helm-diff](https://github.com/databus23/helm-diff) and showed diff for Helm release install and upgrade. It's a very useful tool !!

The annoying part is when we have to parse the Helm diff logs when doing diff / plan and not actually doing the upgrade. We have to ensure that the changes that are happening are actually valid and expected. If there's something unexpected / some error happens, we stop and don't do the actual Helm upgrade

We remember how we had to ignore `metadata.labels` changes at the top level of the objects as label changes never caused any harms, unlike `spec` changes which can cause a pod to be created a new and old ones to be gone, and you may not want that, so you gotta be very careful, or else one may incur downtime due to pod restarting / a old pod dying and new pod coming up

Our tech lead was in fact trying out different solutions to see how we can avoid doing the tedious task of parsing all the Helm diff output by staring with creating a tool to ignore `metadata.labels` changes for starters

That was still at an idea phase, later the team ramped down and people moved out to different teams to do different stuff for other clients

That was one of my stories. Thanks for reading till here! Let me know what you felt in the comments :)
