---
title: "Debugging Issues in Software World"
date: 2021-03-08T11:25:35+05:30
draft: true
---

Debugging issues is a common thing that happens in the software industry. Teams
build software, sometimes bugs pop up and they find out the reason for it and
then fix it.

In 2017, I remember in one of my initial training sessions where our trainer
literally told that debugging is not needed and that if we just follow Test
Driven Development (TDD) and write good tests and follow TDD well, then
debugging won't be needed at all. This happened when there was a discussion on our training batch slack talking about how to use debuggers in IntelliJ IDEA IDE for Java programs.

Well, I don't know about what my trainer said, but I do know that a lot of times I have ended up debugging and using debuggers to help me. For example, I used to contribute to an Open Source Software called Helm. A new major release of Helm was released and it had issues. A lot of small issues. I was working a few of those issues. Most of the time I would just read code and find the issue and fix it. It would take a lot of time though. I have come to realize that sometimes debugging takes a lot of time. I have heard from others too that debugging is the most time taking task. I guess "it depends" :P It's a standard funny answer we used to get in our initial training days from our trainers. Anyways. Reading the code and debugging was hard. Really hard. But it worked for simple small bugs. As time went on, I started checking some really hard bugs. I wanted to understand what exactly was going wrong. I mean, I knew how to reproduce the bug, which is considered to be one of the most important steps in fixing a bug. But I didn't know what was going on in the code and why some weird things were going on and how the bug occurred. I always ended up reading code and executing it in my mind, but that works only for basic stuff, basic data etc and basic issues. Not for really mind bending issues.

Enter debuggers. One fine day, I decided to finally pair with a colleague on one of the Helm issues. We never fixed it later, but I ended up learning a good thing that day. I finally got frustrated and found out how to debug a Golang program with IntelliJ IDEA IDE. This was so cool! At one point, I understood how to key in the Helm command to the debugger's config so that it runs that Helm command and then helps me debug the program. Helm is a command line tool by the way.

With the debugger, I could traverse through the code while using breakpoints,see the exact data when at the breakpoint. I could also keep conditional breakpoints so that I don't have to stop unnecessarily for some particular data. It was so cool!

In my current project I also learned to use the Expression Evaluator that debuggers provide, to run Expressions while I'm paused on a breakpoint. It's damn cool!

Based on my experiences, I can tell a few things about debugging

1. Learn how to reproduce the bug and reproduce it.

What does it mean? Follow some set of steps for the bug to occur. If you
successfully do this and have a reason of why it happens, then after you are
done fixing this issue, you will know if bugs occur or not by following the
same set of steps to see if the bug is still there.

2. Always know how to debug your system / code

This is very important. You need to be able to extract data out of your system.
You can do it in multiple ways. If it's a simple single system, and it runs
locally in your machine, it's probably very straightforward to run it and debug
it. You can use logging, writing to files, anything. Anything to get data out
of your system while it runs so that you can get more information and debug to
understand what's the issue.

If it's a distributed system, it can be pretty tricky. For example, in my
current project we use multiple separate services, each having it's own
responsibilities and they interact with each other with the help of events
based on event architecture. Now, how do I debug such a system? I use whatever
data is present in the system already. For example, it's an order management
system and it's designed in such a way that one can easily find an order by
just using the order ID, and this will always be referenced in all the systems.
So, it's good to have one ID that can correlate across multiple systems so that
you can understand what happened to a particular data which caused ripples
across the system. We use Kafka to send events. I can consume from the Kafka
messages as a unique new consumer and get all Kafka messages from the topic
from the beginning. Also, timestamps are important too. And logs. And any other
data that basically helps you get more information about what's going on in the
system and what "happened" in the system in the past. It's also called as
observability of the system. I believe it's important for debugging.

The other thing about debugging is, you need to know how to use the tool that
helps you debug. Apart from logging, debuggers are really helpful because of the
already above mentioned reasons. It's good to learn about debuggers and how to
use the IDE / text editor debuggers based on the IDE / text editor you. For
example, if you code in Java, you can learn how to use the Java debugger in your
local using IDEs / text editors.

3. Guess / make a calculated guess about what could have gone wrong and try to
back it up with some reasoning - past experiences and observations, factual
data etc.

It's good to have some guesses or calculated guesses about what might have gone
wrong by looking at the code or based on your understanding of the system. Also,
you may have seen a similar in the same project or in some other project. So,
you could use your past experience here.

Write down all your assumptions, and doubts about the culprit causing the issue.

Now have ways to validate the doubts, assumptions. If there's anything that you
find that cannot be reasoned out, that's hard. In any case, always have enough
data about your experiences and when noticing the bugs. The more data about 
this will make things merrier :) More data about the system always helps. How?
Well, usually if a bug is hiding, it's hiding behind some code, some details
about the system that aren't obvious or clear. These are details that are
probably not very transparent to you, and hence you won't be able to understand
what's going on.

So, it's always good to have enough data about your systems, and control over
them too.

4. Use that debugger, log statements etc and debug the code! :D

5. Write failing tests for the issue. Fix the code

This is in fact the actual mantra I have followed most of the time when I
working on fixing issues in Open Source projects
