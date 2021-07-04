---
title: "Time machine series: Part 2 - Curious case of database connection failing"
date: 2021-07-04T21:30:05+05:30
draft: true
---

We were using Kubernetes and Consul for DNS

We were using a tool called kube consul register - https://github.com/tczekajlo/kube-consul-register to register pods as services to consul (through the consul agent / server?)

We realized that there was a bug in the tool where it didn't remove the service once the pod was gone

What this lead to was - the service remained in consul and consul agents kept trying to do health checks and showed failed health checks - basically failed services

This also caused one weird problem - when a pod was gone and there was this zombie corresponding service in the consul, say

abcd-postgres-db service pod was in the consul as abcd-postgres-db service

Later, let's say def-postgres-db pod came up, with the same IP as the previous abcd-postgres-db. Now the consul agents still had abcd-postgres-db service in it's service registry along with health checks along with the IP address. Consul was still doing health checks and the previously failing health checks suddenly start passing. How?! This is because now consul is running the health checks against def-postgres-db actually thinking it's abcd-postgres-db . I'm not able to recall if there was also the def-postgres-db service separately with the same IP, maybe there was and that's a valid entry in the service registry

Now what this lead to was - since we were using consul service as a DNS service, and it's service registry had abcd-postgres-db with the IP address of the def-postgres-db and the health checks were passing - as the health checks simply checked if port 5432 was accessible, what happened was - the services which were trying to access abcd-postgres-db using the consul DNS service now were getting def-postgres-db IP and were accessing def-postgres-db but then they use the abcd-postgres-db credentials - which don't work in def-postgres-db and hence the connection fails

We fortunately noticed this and fixed it

The craziest part is - had the credentials (for example username and password) of all the databases had been the same, you would have two services using the same database and possible crazy issues because of that - performance issues, lots of storage usage and the biggest problem - corruption of data in case both use the same database name and table names for some reason. Most probably these won't happen and fortunately things stopped at the authentication level itself for us, but this was still an embarassing issue! 🙈😅

Let me give you a gist of what was the most craziest part. Our redis databases had no credentials. Almost none of them had it, maybe a few max. Most others just needed the IP/hostname + port number for connection, no credentials. So if there was a ghi-redis-db and a jkl-redis-db , and if there was a similar consul service registry entry issue where IP of an old redis pod say ghi-redis-db pod gets reused in a different new redis pod say jkl-redis-db, then any service using ghi-redis-db might be connecting to jkl-redis-db too and it will not have any issues connecting to it!! As we always used port 6379 and there was no credentials and for host/dns name/ip there was consul where the above issue could occur and that's it, a service might corrupt the data in another service's redis db!!!

It's very very important to ensure that the DNS service is proper and of course, it's better to secure any database with proper credentials and of course not reuse the same set of credentials for different databases. And also try to rotate credentials - use something like vault maybe, and ensure that a service has access only to the credentials of it's own databases / data stores, that is, data stores it interacts with and has access to. It's also good to have authorization / access levels - read/write and any possible granular access levels depending on the data store usage

