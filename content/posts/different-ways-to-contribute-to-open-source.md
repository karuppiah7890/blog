---
title: "Different Ways to Contribute to Open Source"
date: 2020-01-29T17:40:02+05:30
---

This is not an exhaustive list

1. Donate Money
2. Write code - bug fixes, features, security issue fixes, performance improvements
3. Write docs, Fix docs
4. Simply use the software and provide UI/UX feedback and propose different UI/UX by filing an issue
5. Report bugs that you notice while using the software
6. Check existing bugs in issues and help by reproducing the issue if no one has confirmed that it's an issue with the software. Usually people confirm it in comments or by labelling it bug. Sometimes people label it bug, even if it's not confirmed. It really depends on the project and how they work 😅
7. Debug issues - first step to fix a bug is to debug it and find the root cause. Use print statements and logs or debugger to find root cause. After finding root cause, you may or may not find it easy to fix the issue. Like, it's possible you don't know why it's happening but know which part of the code is causing it. Put all the debugging details into the issue as a comment so that someone looking at the issue might find it useful to work further on it. I have done this a few times. I did this when I tried to fix the issue, but found it hard to comprehend. But I could find the root cause, so I just posted that
8. At some point you will know you won't be contributing to the project for a long time, you might wanna try some other projects. Or you might wanna just continue with the same. In any case, one awesome thing you can do is, mentor and help more contributors to contribute. It's basically scaling contributions. As a single person, you can only do so much. Now think what multiple people can do, if you help them contribute like how the maintainers helped you. Different ways to help - after you have some context on the project, you will know some of the issues that are very easy to fix. DONT pick them. Let some new contributors pick it up. Also, usually there's no authority or hierarchy. Everyone is usually very friendly. So, you are basically helping friends. If you find bad conduct, report to maintainers for further action. That's a different story.
9. Anyways, so, now you have left the easy issue alone, is that enough? No, not exactly. It's a good start, there are other things too. Now the problem with easy issues is that, many people try to fix it. And open source also has communication issues. Some people not communicating that they are working on fixing an issue, and then raising PRs directly, and multiple people doing that - it's basically wasted effort because only one PR will get accepted. Unfortunately there's no one silver bullet to this. But have this in mind and help people communicate. Also, if someone says they are working on something, and someone new comes and says they also work on it very soon after the other person, you can help by saying that the first person could try it. Also, there might be cases where a person might just block the issue by saying they will work on it, and they might even get assigned. It's good to get help from maintainers to decide what can be done when there's an issue that's been blocked and with no progress. It happens. Unfortunately I have done it too a few times. So these days I don't commit very easily these days. I first analyze and see how much time I have and then commit. Recently I have committed to very few and very small issues only
10. Now once the PRs are created for these easy issues, help review them. Some basic things that I did was - do small and simple checks - like check if the pipeline for the PR is passing, check if there is test code for the PR - always advocate for test code. And then you can read the code and give your opinions. And remember, be friendly. The other person is just trying to help the project by spending time and effort. For tests, I know you all know it's very important, I still want to give one example from my experience - so, there was one issue in helm for which a person raised PR and fixed it. Now he didn't write a test for it and it was actually a very very small and simple fix. Now, later, like quite after some time, someone else was working on fixing something else and changed the fix code of the other PR and this introduced the old bug again. Now the person who fixed that old bug came and reported again. Now this time I saw this and said that tests can help prevent this. He thoughtfully wrote a really good test that the maintainers appreciated, but I didn't see it, mostly because I had no idea about that part of the code and so didn't check. But the good thing that happened is, the next time someone changes that fix code, the tests will fail and we'll know. Always advocate for tests for bug fixes, security fixes and features too. For performance - Idk if people write test and how they do. I know people do benchmarks. Not sure if people have a test to make sure performance doesn't go down the previous one. May be someone talk about that sometime, if they know about it
11. Pair with people in office and contribute. This helps some people to get started. A lot of people just have trouble in getting started. But if you pair once or a few times. Then they might get comfortable with contributing and will be on their own. This is also one way to scale contributions from a single person, that is you, to multiple people. 
12. Talk about open source when you talk tech. This is anywhere - just random tech conversations, or a session like this, or meetup talk, or a workshop, or a conference. Anywhere. Just talk about open source project - it's basically creating awareness and yeah, marketing :p
13. Write tutorials in your blog on open source projects
14. Create video tutorials about open source projects
15. Write experiences about open source and open source projects in your blog. (This can be a general thing, or about a particular open source project)
16. Answer questions about open source and open source projects in the community. Many projects have its own community - which includes physical meetups and digital forums too - like slack, discord, discourse, and more. Open source in general also has lot of general meetups, conferences, and digital communities where people talk about multiple projects from different domains


Just ending this with a thing - did you know there's open source hardware? Did you know there's open source art? Also, I'm thinking open source knowledge could also be a term. I mean, most of the Internet is just open knowledge! Blogs, articles, videos, what not. 

By the way, a lot of this was based on experience, and for many open source
contributors it's kind of obvious I think. Generally, I believe that - anything
open source we do, that is, anything related to open source, then it's contributing
to open source. 

Some (or all? I don't know 😅) of these are also mentioned here - 

https://github.com/openfaas/faas/blob/master/CONTRIBUTING.md#how-can-i-get-involved

I think the above is the only place where I saw a lot of diverse ways
to contribute, unlike a lot of other contributing.md files
