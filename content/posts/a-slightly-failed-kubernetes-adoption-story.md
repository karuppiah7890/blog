---
title: "A slightly failed Kubernetes adoption story"
date: 2021-04-03T11:23:35+05:30
---

Let me start with the beginning of the story of the team when I wasn't present. I know only some details about this which I was told by the team members. I know better about what happened after I joined, but given my bad memory, I can only recall some of the important moments. Also, it was around a 2 year journey. I'll try to recall as much as possible 😅

Before I joined the team, we were approached by our clients to work on a project to help them expand to multiple countries. The idea for them was to fork the infrastructure they already had for this expansion instead of using the existing infrastructure. I'm not sure about the reasoning behind this decision but apparently this is what they decided for expanding to some of the countries.

And when they wanted to clone, they said that they don't want to clone the whole existing infrastructure but clone only parts of it - as they wanted to expand only some of their services to other countries and not all at once.

Their existing infrastructure was on GCP and was running VMs with many open source and custom automation tools. For example they used chef for some of their automation.

The client was also moving from a monolithic architecture to a microservices architecture.

When our team went in for the task of cloning, they decided to use Kubernetes to deploy the necessary microservices, including the monolith that was still in the process of getting split slowly into multiple microservices.

I think one of the main hurdles initially was getting context about what microservices are required to provide the customer services that the client wanted to expand to the new countries. With some collaboration, the team got to know this I think. I also remember the team mentioning how they drew an architecture diagram to understand the dependency among the different microservices just by checking the configuration of the microservices - as the configuration had the host of other microservices if they were dependent on them.

The team finally started working on the clone. They successfully deployed the microservices in the new cloned environment which was completely based on kubernetes.

This was the time I joined the team. I noticed a lot of Quality Analysts testing and helping us to understand if everything was working on the new environment so that we can go to production when everything is good.

Apart from microservices, we were also running Postrgres and Redis and few other components in the kubernetes cluster. In the existing infrastructure too, managed services were not being used much, instead Postgres and Redis was deployed on VMs using Chef cookbooks.

All was going well. But if you notice one thing, I never spoke about the teams building the microservices.

Most of the time, our team was not talking to product teams so much. Surely quality analysts were working with product teams and with us too. We were more of an infrastructure team and we were not the only one, there were other similar infrastructure teams with their own projects.

This is the tricky part, we did most of the work with little to no communication with the product teams. Surely the product teams knew about this expansion to new countries. They were slowly building new features like internationalization to support the new languages required for the new countries. Sometimes the microservices didn't work for some reason, we would then get help from the product teams, or check out the issue ourselves based on the logs and see what kind of issue it was. If it was infrastructure issues, we resolved them easily as were the ones setting up the infrastructure.

We were going at a very high speed with respect to deployment and automation. The client management was very happy with the work and was also trying to push the expansion as soon as possible.

The only problematic part was - every time we had to do something, we tried to avoid communication with teams as much as possible. Why? As there were so many teams, communication and dependency on product teams would mean friction and more time to go live and finish the expansion. So, our team was always trying to avoid communication and dependency on product teams. How did we do this? Let me give you some examples

The client had multiple custom solutions for the same kind of problems. They quickly realized this and mentioned that some solutions are old and need to be decomissioned and that they have newer methods but there's not much adoption for the newer solutions. Our team got into this project of migrating from older systems to newer systems for configuration management and devised a plan and overnight this was done, without the help of any product team. The idea was to keep the migration as robust as possible.

The configuration management system migration was mostly successful with small problems. The teams that faced problems reached out and it got fixed. But there were some people issues. So, what happened? Some teams literally started saying things like " Why was this migration forced on us? We didn't choose to do this migration. Also, why such a big bang migration over one night? ". There were some announcement emails around this I think. I was still new to the team so I was still trying to make sense of it all. But yeah, some teams were pretty pissed at what happened. But finally, everything was discussed and problems were solved.

Similar to this, our team did multiple small migrations. For example, we migrated the CI/CD pipelines of all the teams - all their services, to deploy to the new environments apart from deploying to VMs. We got the help of the client teams who had already built automation scripts and systems to help with kubernetes deployment. It's just that there wasn't too much adoption. So, we got into the project and with very little communication with teams, we went ahead and added the deployment to kubernetes clusters too.

We did all this kind of stuff so that teams deployed to kubernetes cluster too, the new environment, apart from VMs, their existing environment. And there was configuration for the new environment too. We also worked on ensuring there is not much drift in service version and configuration. Drift - lot of difference. By removing the difference, we ensured that the services in the kubernetes environment were up to date and running well, which was important, or else they were out to date and sometimes were not running.

Again, the problematic part is, very little communication with teams. Very less involvement of teams in the whole process. What did this lead to? Well, very few teams and team members understood what was going on in the new environment. And we went live after some point! Mind you, even after this, some of the teams were having trouble understanding their new environment. But the teams were more interested and responsible to take care of the environment now since it was live and in production, unlike before

Some of the things that happened after we went to production was - two kinds of problems - one was - people problem - many were still not okay with some of the decisions with respect to kubernetes. For example, many didn't like the fact that Postgres and Redis was running on kubernetes. For some, there was the problem of gap in knowledge when it comes to kubernetes, but they knew a lot when it came to VMs.

Teams were very comfortable with maintaining and running their existing infrastructure which was on VMs, but in some cases, struggled when it came to their new kubernetes environment.

Our team had worked on automatic access management automation. Teams used this to get access to the clusters slowly and then understand their infrastructure.

They started to work on production issues whenever they happened. They slowly started to get a hang of it.

Some of the reasons why some people didn't like kubernetes - I think it was partially because of our team. We had some small / big issues in our kubernetes clusters

The first issue was - we were using GKE and it came with it's own issues. I remember we suffered a lot because there was some issues in a GKE load balancer which allowed traffic to a not-ready API server instance during GKE master component upgrades which includes API server upgrades. This lead to our worker nodes going into a not-ready state as the communication between kubelet and API server was affected because of the load balancer issue. Not ready worker node meant pods getting rescheduled and causing a lot of havoc especially when we were running databases on kubernetes too.

This was one reason people didn't like running Postgres, Redis on kubernetes. In kubernetes, pods don't have so much guarantee - they can go down any time for various reasons, and in our case it happened a bit too much. In the VM world, VMs are pretty stable. So product teams felt Postgres and Redis can run in a more stable manner on VMs, and they can sleep well at night instead of working on production issues caused by some issue in Postgres, Redis.

Another thing with Postgres in Kubernetes was - Postgres is not exactly meant to be a distributed database. I remember this one issue were a primary Postgres server shutdown (pod issues) and a replica / secondary became primary. This was possible because we had high availability (HA) setup for Postgres using an automation tool called Stolon. Now, when this switch of primary happened, the problem was - the previous primary had a lot of cache, but the new primary had no cache, so the queries were performing slowly. This caused alerts in the services.

Basically, a lot of havoc, and people lost trust in the kubernetes platform due to various such instances.

A few issues from our side was - initially we made one mistake of not taking care of the IP addresses used in the cluster. At some point we realized that the private IPs we use clash with the private IPs used in another kubernetes cluster and the problem was - we had to connect and communicate with this other kubernetes cluster. We finally solved the problem by creating a new cluster in GKE and this time used the client's new automation tool that they created to allocated private IPs to multiple entities within the organization so that in case any of them want to communicate with each other at some point, there won't be any private IP clash. It was a solution similar to IANA but for private IPs, and a custom solution.

Since we had to move to a completely new cluster, it was a big migration. And it was just after going live in production. It was very scary. Had to ensure minimal to 0 downtime, as always. We did it with lot of planning.

What other kind of issues did we face?

We faced pod resource hogging issues. Some services had memory leakage issues, or needed more resources but we hadn't configured minium or maximum resources for the services at the pod level. So, they had unlimited resources in some cases. This lead to some pods affecting other pods running in the same worker node by hogging resources.

For memory leakage issues, teams didn't have the bandwidth to find the issue. We decided that till the team finds the issue, we will kill the service every 24 hrs or so using a cron job, so that the service doesn't use too much memory. It was a stateless service, so this was an okay solution and it didn't harm anything

We faced the IP issues again. But this time, it was about how we had very less IPs to use in the cluster, which meant having a small kubernetes cluster - with less number of kubernetes nodes. But we actually wanted more nodes as we wanted to deploy more and more services as the client wanted to slowly deploy all services in the new environment as part of the expansion, to provide all customer services. To avoid the small cluster size problem because of small IP range provided for the cluster, we had to create more clusters and this time, we had a bigger IP range though, so that we don't have the same old problem again

Apart from these kind of issues, we were also faced with a lot of support requests from product teams. They wanted to get help to get onboarded to the new kubernetes environment and to deploy their new services by themselves. This is the time I kind of realized that modern software development is pretty complex. We had documentation but it was still not easy for some of the product teams to understand it or make sense of it. There were probably multiple reasons, but I guess the main reasons were - custom tools with some complexity in learning them and using them, and kubernetes being a not so great platform for application developers to interact with directly. But the product teams were forced to do that, and many other teams around the world do it too. There's even a certificate for this thing - certified kubernetes application developer. You can imagine how much you need to know to work with kubernetes as an application developer. Product teams didn't have the time to put effort and learn kubernetes I guess, I mean, it was all so fast and they were always building features, or supporting the production issues in different environments - VMs or kubernetes. I think a lot of their learning was on the go when it was needed.

Due to all this, we had lot of support requests for various things - help with onboarding a service, a new database, a new redis, in the kubernetes environment. Or help with some issue in the kubernetes environment.

Something to note is, we were a small team, a single team, and there were so many product teams. Given that we took the route of going to production with almost 0 help or involvement of the product teams, now the product teams didn't go through the journey we went through and they were left behind and they were struggling to get things done as part of their daily development, and had to depend on us as we had lot of knowledge around the whole setup. Initially lot of support requests came through slack and email. At some point we were using the classic help desk / ticket method with the help of some SaaS services to manage the enormous amount of support requests.

Whenever someone from the product teams struggled and pinged me for help, I would feel guilty about the platform we had created and the issues it has and how we don't have the time to help the product teams who were suffering. I also knew some of these product teams personally and felt really bad for the kind of things we built - things that were not so great for the end user. End users were literally having bad days because of the whole situation we were in.

Lot of the support requests was also around performance. There were some performance issues like network latency or performance of the DB not being as good as the VM counterparts that the teams managed. Most of the time it was some resource hogging issue, and for network - we came up with solutions to solve multiple network hops which caused latency. We also tried to reduce DNS resolution time which was also a bottleneck. The product teams were of course very smart to find all these issues as they had already found all these issues in their existing infrastructure and fixed it themselves. But that was all in VM world. We were using kubernetes which runs on top of VMs and has it's own way of things and product teams struggled a bit with this and had very little time and control and take it up in their own hands and fix it.

All in all, most of the time, the teams were struggling to manage their services that were on kubernetes. They felt powerless at times - either due to knowledge gap issues. But like I said, they were trying to learn but didn't have the time given the amount of time they were spending on building features for their services and also maintaining them in production in all the environments. I remember how some of them worked day and night almost all the time and were on slack replying to threads and discussing issues. Some really seemed like super heroes, but in reality, they were overworked.

Finally, at some point, the teams said that they want to move their databases to VMs and not use kubernetes. Many teams started doing this slowly. The management also took some decisions on this and said - services will run on kubernetes but databases will run on VMs, as they felt VMs were more stable for databases and other such stateful components

All in all, I faced and experienced a lot of scale issues and learned from my team on how to solve them. But I also had lot of sad experiences where I couldn't help the teams with the time I had, and the same applied to my team. We were stuck in between a lot of things - building features, support that we couldn't manage. It was a hard time.

In an ideal world, I would have expected that we took all the product teams on a journey to using kubernetes and running services on kubernetes, instead of us doing it completely for them. The only problem is that it would have been slower I guess and management wanted speed. In fact someone told us how we followed a lot of anti patterns and still saw success (in the eyes of client management), but agreed that the product teams struggled.

I just wanted to talk about the failures in this whole journey. Surely there were lot of wins. Like going to production, doing a lot of things with little dependence on the product teams, migrating a lot of old systems to new systems in a smooth manner. It's just that, I wanted to tell about how sometimes with all these wins, there are still some failures that have lot of impact on the end users. In our case, the end users were developers. Being a developer, I could see the struggle of the developers whom our team was supposed to serve but our team was serving the client management and not the client developers and product teams.

Client management also could have handled this well and took the product teams on a journey. Client management could have got the green light from teams to agree on the different decisions they took so that they could get the help and commitment of the product teams. But in my eyes, most things looked like " We are management, we are taking this decision, just deal with it ". Until product teams retaliated a bit and then things took a turn and management also understood that things have to change. The client was a very technology oriented client, and a lot of them were engineers first than managers and the engineering heads finally favored the product teams of course and helped the product teams, who were the end users for teams like us.

I hope you learned a thing or two from this whole story. I guess it was a big roller coaster ride, talking about different issues, but that's how I remember the story based on my memory, so, I apologize if it looks a bit chaotic.
