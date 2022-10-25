---
title: "Rate Limiting Basics"
date: 2022-10-25T10:44:30+05:30
---

In this post, I wanna jot down my thoughts on rate limiting while I ponder about it and learn more about it and understand the why, what, how

So, what's rate limiting? Before going there to answer that question, let's see a scenario that can happen in the realy world

Let's say you build a service. It's a long running service that responds to network requests, for example HTTP requests to an HTTP API served by your service

Let's say your service gets lot of requests from different users, through various HTTP clients (CLI tool, Browser, another service)

Now, in this scenario, it's possible that your service is getting lot of requests in a given day, or even at a given hour, or minute or second. Now, your service is running on a small machine (in terms of resources and size), which has only some amount of resources (CPU, RAM, Disk, Network bandwidth). It's clearly not infinite, it can only serve some amount of requests at a given period of time. Which means, due to this, the service cannot serve many users at once / at a given time period, depending on it's performance, limitations, implementation (number of CPU cores in machine, usage of different cores etc), available resources. What this means is, the service cannot scale and serve as many users as possible when more users come in. Let's say the scale of requests increases gradually. at some point the service and the users of the service are going to struggle - 
- The machine could become slow
- The service could become slow to respond to some or many requests
- The user might see high response times - for example, in orders of seconds, which is not great
- Key metrics of the service might show bad values. For example, 99th percentile value of response time might be really high than the usual values, when there are a lot of requests / unmanageable number of requests

Now, in this case, you could do some things. Of course you can scale the service - run it on bigger machines (vertical scaling - scaling up), or run it on many machines (horizontal scaling - scaling up). Also, if the infrastructure is elastic - when number of requests are high, you could scale up, when the number of requests drop down, then you could scale down to smaller machines (vertical scaling - scaling down), or lesser machines (horizontal scaling - scaling down)

One thing to note here when talking about scaling is - what's a reasonable and real scale vs unreasonable / unreal / unacceptable scale. Like, I can't say "Hey, this service of mine should be able to scale and handle infinite number of requests. It should be able to scale as much possible, using as much compute available in the world in different data centers around the world". I mean, I could say, but is it feasible to implement is a question to ponder about and also, do people design for such use cases is also a good queston to ponder about, where people use terms like "infinite" without proper and finite numbers like "10K requests per second" etc

Also, another thing to note is, there are lot of "bad entities" in the world. Like bots, scripts, programs, that simply hit (send requests to) services and try to spam them, to make them unavailable to genuine users / to corrupt their internal data etc. For spamming, "Denial of Service" attacks (DoS) and "Distributed Denial of Service" (DDoS) attacks, people do many things to detect spammy requests and block them - block the IPs for a period of time etc. For such cases, it's important to prevent the service from getting spammed / misused by some clients/users, by detecting spam - for example based on service usage - like number of requests per second, kind of requests / behaviour etc

Also, some folks open up their APIs and provide their APIs for a fixed cost (paid services) which has a corresponding fixed API limit given a time period. For more limits, you pay more etc. Some other services expect genuine people to use their API but only to a good level, so that it doesn't get abused, affecting other genuine users using the API. For example, GitHub has a limit on the number of API calls done per second to the GitHub REST APIs for authenticated and unauthenticated users. GitHub refers to the rate limiting details in it's Rate Limiting API docs - https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting , https://docs.github.com/en/rest/rate-limit

Now, how does one implement rate limiting, for ensuring fair usage? Let's say a Service S can take 1000 requests per second. Let's say User A sends 1000 requests at 10:01:30 am and User B also sends 1000 requests at 10:01:30 am. Now, it's very possible that 700 requests of User A went through and got a response in that second and 300 requests of User B went through and got a response. The remaining requests went through later and got a response later. This might be all dependent on the timing at which the requests came (at nanosecond or picosecond level) and the speed of the users and the service. Now, let's assume there are around 100 users for this service. Now, how would one go about allowing fair usage of this service?

One idea is to allow 10 requests per second per user so that even if all 100 users use the service, the service can handle 1000 requests per second. You ask how? A rough calculation is - 

(10 requests per second per user x 100 users) = 1000 requests per second overall at system / service level

This calculation is based on the idea that - all users are equal and we know the exact number of users and how much requests the service can handle. When all users are not equal according the system - say some users are using trial / free tier of the API, some users pay a small cost, some users pay a very high cost, then the service can have different rate limits for different users - whoever pays more can have higher rate limits considering they are "premium" / "paid" users

Now, how does one go about implementing this? Say in an HTTP API. How will the service stop the 11th request from a User A in a given second? In HTTP APIs, there is no state, as it's a stateless protocol and when there's a request, how will you know who sent the request to keep the count of requests per user / client in this case? There are two ways - one is - get the IP address of the user / client. Another is - have authentication features in the API and ask the user / client to authenticate with the API using a secret (API token, private key etc) and then use the API, this way the service can then identify and differentiate users / clients by using the authentication information in the HTTP request

So, now you have an idea of how one would go about implementing rate limiting at a basic level and how to differentiate between users / clients of the API and how basic rate limiting strategies can be implemented at a high level

I'm writing this blog post to improve my own understanding of rate limiting and also explore rate limiting in web servers / systems online, for example Kong API Gateway (Open Source) rate limiting plugin, and advanced rate limiting Kong plugin
