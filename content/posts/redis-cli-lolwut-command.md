---
title: "Redis CLI lolwut Command"
date: 2020-03-06T11:16:25+05:30
---

So, today I found something funny and interesting. Someone in our slack `#tech-reads` channel shared
a tech read by `antirez`, the creator of `redis` software. This is the link that they shared - 

http://antirez.com/news/123

I immediately tried out the command in my terminal and it was cool! See the recording here -

{{<asciinema 307912>}}

The commands used in the recording are below 

```
$ redis-cli
127.0.0.1:6379> lolwut
127.0.0.1:6379> lolwut 10 1 4
127.0.0.1:6379> lolwut 1 1 4
127.0.0.1:6379> lolwut 1 1 5
127.0.0.1:6379> lolwut 10 1 5
```

Try it in your local if you like it ;) :D And do read the post - http://antirez.com/news/123 :)
