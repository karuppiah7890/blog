---
title: "Time machine series: Part 2 - Curious case of database connection failing"
date: 2021-07-04T21:30:05+05:30
---

In this time machine series blog post, I'm going to go back in time and recall an interesting problem we faced. We faced many interesting problems actually. I'll try to cover each one of them in a time machine series blog post :D For now I'm just noting down all the bits and pieces in my notes based on whatever I can recall!

Now, let's checkout what problem we faced with respect to database connections!

So, we were using Kubernetes for running our client's microservices or any stateless services. We were also running databases / any stateful components in Kubernetes clusters. We were using [Consul](https://www.consul.io) for the DNS of stateful components as they were in a different cluster and not in the same cluster as the stateless services. If they were all in the same cluster, we could have just used the Kube DNS, but that wasn't the case though

Now, To integrate stateful components running in Kubernetes with Consul, we were using a tool called kube consul register - https://github.com/tczekajlo/kube-consul-register to register pods as services to consul service registry

Later at one point, we realized that there was a bug in the tool where it didn't remove the service once the pod was gone. Ideally the tool had to keep the service registry up to date with the service name and the correct list of IPs - pod IPs. What this meant was - if a new pod came up for the service - that should be in the service registry for that service, and if a pod dies or goes away for a service, that should be removed from the service registry for that service. Only then the Consul service registry will be up to date and only then the Consul DNS service can respond with correct results and no stale results, especially in a very dynamic environment like Kubernetes where new pods can come at any point and existing pod can die at any point, and pod IPs are all dynamic and a service may not get the same pod IP as before when one of it's pods goes away and a new pod comes up

What this lead to was - in such cases, where a service's pod goes away, the service record with that old pod IP remained in consul service registry and consul kept trying to do health checks using the registered health checks and showed failed health checks - basically failed services. This was because there was no pod with that pod IP, so any health checks would fail. Ideally that pod IP should have been removed from the service when the pod died

This also caused one weird problem in a specific situation - when a pod was gone and there was this zombie corresponding service in the consul, for example say

service-abcd-postgres-db pod IP was in the consul service registry under service-abcd-postgres-db service, but service-abcd-postgres-db pod died but the dead pod's IP still remained in the service registry under service-abcd-postgres-db service

Later, let's say service-def-postgres-db pod came up, with the same IP as the previously dead service-abcd-postgres-db pod IP. This is possible as the pod IPs are all dynamic and any pod can get any IP and IP could be reused too if it's not assigned to any pod currently, so there's no rule stopping that, at least in our case we didn't have anything like that. Now the consul still had service-abcd-postgres-db service in it's service registry along with health checks along with the IP address. Consul was still doing health checks and the previously failing health checks suddenly start passing. How?! This is because now consul is running the health checks against service-def-postgres-db actually thinking it's service-abcd-postgres-db . I'm not able to recall if there was also the service-def-postgres-db service separately with the same IP, maybe there was, I mean, ideally it should have been present, but that will however be considered as a valid entry / service record in the service registry, unlike the wrong pod IP under service-abcd-postgres-db

Now what this lead to was - since we were using consul service as a DNS service, and it's service registry had service-abcd-postgres-db with the IP address of the service-def-postgres-db and the health checks were passing - as the health checks simply checked if port 5432 was accessible, what happened was - the services which were trying to access service-abcd-postgres-db using the consul DNS service were now getting service-def-postgres-db IP and were accessing service-def-postgres-db but then they used the service-abcd-postgres-db credentials - which didn't work in service-def-postgres-db as service-def-postgres-db had a different set of credentials and hence the connection failed

We fortunately noticed this and fixed it! This was based on some reports by some teams on intermittent issues regarding connection failures and also some failed services showing up in consul

The craziest part is - had the credentials of all the databases had been the same, for example the same basic username and password, you would have two services using the same database and many possible crazy issues because of that - performance issues, lots of storage usage and the biggest problem - corruption of data in case both use the same database name and table names for some reason. Most probably these won't happen - different services usually use different names etc, but still, it's a possibility in theory!! Fortunately things stopped at the authentication level itself for us, but this was still an embarrassing issue! 🙈😅

Let me give you a gist of what was the most craziest part apart from that. Our redis databases had no credentials. Almost none of them had it, maybe a few of them had, very very few. Most others just needed the IP/hostname + port number for connection, no credentials. So if there was a service-ghi-redis-db and a service-jkl-redis-db , and if there was a similar consul service registry entry issue where IP of an old redis pod say service-ghi-redis-db pod gets reused in a different new redis pod say service-jkl-redis-db, then any service using service-ghi-redis-db might be connecting to service-jkl-redis-db too and it will not have any issues connecting to it!! As we always used port 6379 and there was no credentials and for host/dns name/ip there was consul where the above issue could occur and that's it, a service might corrupt the data in another service's redis db!!!

## Learnings

It's very very important to ensure that the DNS service is proper - Of course!

Of course it's better to secure any database / any service with proper credentials and of course not reuse the same set of credentials for different databases / services. Also one could try to rotate credentials - use something like [HashiCorp Vault](https://www.hashicorp.com/products/vault) maybe, and ensure that a service has access only to the credentials of it's own databases / data stores, that is, data stores it interacts with and has access to. It's also good to have authorization / access levels - read/write etc and any possible granular access levels depending on the data store usage. One could extrapolate the same idea from data stores to any external service access and not just data stores

