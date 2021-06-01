---
title: "Modern Software Development"
date: 2021-06-01T20:54:50+05:30
---

This post is to see some of the cool things in the modern software development and the kind of experience that developers can get. This post especially focuses on a kind of experience that I expect to get as a developer when I develop for backend (servers, services, microservices etc) or frontend (web app). I'm not going to be focusing on other kind of software in this blog post

# Focus

If you think about it, as a developer, I want to focus on building software. Especially if I'm working for a business, then I need to focus on building features that benefit the business

Let's see an example of what possibly happens in the journey of modern day software developer, for example me ;) This is all an example by the way

I read the information about the feature to build for the backend service. I ask any questions that I have. I start designing and then I open my IDE and start building the feature with tests. Now, I want to push the code. I want to run tests, and all kinds of verification - linting, scanning dependencies to look for vulnerabilities and what not, and then run the build process for the software. I can do all this before I push so that I get faster feedback while developing itself. I can then push the code and then let an automated system like Continuous Integration system do the same set of things (test, lint etc) again or more, just to ensure there are no "It works on my machine" issues among many other things

I missed to say that I also try to run the software locally, to test it manually once. How do I do it? I simply use the standard programming language tools or use community built tools and do it. Some of my team mates use Docker sometimes. Notice the keyword being Docker :)

# Deploy

And where do I deploy my backend service? Me and my team deploy the service in Kubernetes. One of the most famous platforms on the Internet

Now, as a developer, does it matter to me as to where my service is deployed? Or how it is deployed? I mean, the tools used to deploy it, and what not.

In a lot of companies, most people would say "Developer owns everything". There's good reason for that and I understand where that's coming from

Let's look at a kind of experience where the developer doesn't worry or think about where their service is deployed. Again, let's consider me as the developer, as an example

Now, I want to deploy my service. How do I do it? I read the docs, I see that I need to go to a link, and then login. And then give details about my service, like, where it's located - the git repository, what are it's artifacts - like JAR file, exe file. There's more details I need to give - like what command is used to start the service, how many replicas / instances to run, if I need auto scaling and if I need a database like Postgres, or a cache like Redis. It also asks me where the service configuration is present, and more inputs like resources (CPU, RAM) required

Finally, after a few minutes, my service is up and running and I can see the logs and status of the service in a dashboard

Did that seem like a long process? Well, a more smaller process would look like - push code to a repository and it will take care of automatically deploying the service and auto scaling and what not

This is the kind of modern software development that's already happening in some companies

# How?

So, how is this possible? What is this called? Generally the service which says "just give me code and I'll run it and help you manage it" is called Platform as a Service (PaaS). PaaS is possible because we have come a long way in software development and solved a lot of problems, though many are reinventing the wheel unfortunately. Ideally, I expect developers to just be able to code and not worry so much about their services. Of course they should be able to observe their services, get alerts and be able to see logs and also debug their service, when issues occur. But developers really don't have to worry about what technology is used to deploy the code they write

# Why?

Why am I talking about this in this blog post? Well, I have been part of the Kubernetes community for some time now, and till date I see people struggling to get through and learn Kubernetes. I understand that for most of these people it's not a choice. If someone asks my opinion, I would simply say that - Kubernetes is great for operators, but it may not be the best interface for developers. Developers usually end up learning lot of things in Kubernetes, just so that they can work with it well. What's an alternative? Developers can try to use tools and systems on top of Kubernetes. There are also PaaS platforms on top of Kubernetes which can provide a good interface for developers

Another thing I have noticed is - since developers deploy their services to Kubernetes, they also do development on it. Or do development on Docker. This is absurd sometimes. Why? The thing is, developers don't really need Docker or Kubernetes to run their service, unless their service is something related to Docker or Kubernetes. In any other case, they can simply run the software natively on their developer machine. Only when thinking about deployment, they need to think about how to create container image, how to configure Kubernetes yamls among other things, to deploy their service. Ideally this has to be taken care differently, using a platform / systems / tools, instead of letting the developer struggle with it. I notice many people asking questions on how to run their service on Kubernetes with hot-reload by mounting source code and what not. It's a tedious thing to do according to me, but people end up doing it

If you look closely, developers are unfortunately mixing up development and deployment. It shouldn't matter if the service is going to run on a VM or a container. In such cases the source code doesn't change, but the deployment configuration may change, yes. There are also other platforms and technologies for deployment, for example Serverless platforms and technologies are there too, in which case source code is a bit different. But at the end of the day, development can be done with ease regardless of how deployment is done. But currently I sometimes see some or a lot of tie up among development and deployment which seems like a struggle for developers

# Conclusion

Software and technology is improving a lot. I'm looking forward to the day when developers just focus on building features and growing business and not spending time understanding Kubernetes or similar platforms where code is deployed. It's also important to understand that Kubernetes is only a platform, and no one knows what platform will be popular in a few years or in a decade's time. Not to mention, some people are already loving the "no-code" services

I'm looking forward to the new developments in the software world and how it makes the life of a developer or any person working in the software industry easier
