---
title: "Developer Experience Part 1"
date: 2021-05-07T14:13:35+05:30
draft: true
---

I have been working with software since 2011 when I started learning the ABCs of C/C++ in my 11th grade, and I was a user of software way before that. I have been professionally working with software since 2017 when I started freelancing and then joined my first company for a full time job

If I look back at all those years of writing code, I can clearly remember some experiences where I banged my head to solve some problem and figured out a good way to debug or analyze the problem at the end of it. 

In the modern day software development, the experience of a developer, usually during development, is called developer experience. Pretty straight forward word huh? :P It also has a short form - DX - Developer eXperience. It's similar to UX -  User eXperience. One can also think of DX as more of a specific domain in UX where the user is a developer

All those crazy experiences I had, it was just a bad developer experience, probably due to various reasons, starting with - I didn't know how to debug an issue with a debugger to many other complicated reasons

In general I have come to believe that DX should be good. It should be nice. It's similar to how businesses and software teams want to provide a good user experience for their users. I think the same amount of importance needs to be given to the DX of developers in the software development team.

What does it mean to have a good DX?

Generally teams or the industry defines some practices as "good practices" or "best practices" and some of them as "bad practices" or "anti patterns". I think it's really the team's call to choose what they define as good or bad based on their context.

Given the team has defined what's good and what's not so good, good actions should have a rewarding experience. A smooth experience. Let me give you an example, let's say writing tests, running tests are good actions for a team. It should be easy for the developers to write the tests, run the tests. If they don't have a good experience writing or running the tests, then inherently they will shy from doing it. If they are forced to do it, then things might just get worse - they are forced to do it, so they do it, but they just hate doing it. In this case, ideally I would expect your developers to want to write and run tests. They need to like to do it to do good work. Or else they might just do it for the sake of doing it.

Not to mention, a bad developer experience can also affect productivity according to me. For example, let's say it's hard or complicated to write tests for some reason. The developer is going to spend more time writing the tests. Same goes for running the tests, let's say it takes a lot of time to run the tests or that the tests are running but they are flaky - they fail sometimes and pass other times, with no change in code. This can just frustrate the developer and also take a lot of their time when running the tests.

A frustrated developer is gonna be losing energy because of their frustration. Energy that they could have instead used to be energetic to do some other work or just have a clear mind to be able to do future work with a clear mind

Frustration, anger, sadness, or any such bad emotion of a developer caused due to a bad developer experience is not gonna help your software development team.

In an ideal case, for the above example of writing and running tests - when developers write more and more tests, they should be able to run these tests with ease and also run them as fast as possible. Some tools also help with running only the tests related to the changed code and not run any other tests by using the concept of caching. It has it's own pros and cons of course.

Based on the above example, I do agree that it's also kind of the duty of the developer to write tests that run fast and are smooth. Sometimes the tools running the test can also help, apart from developer also putting in all the effort to keep the tests lean and fast and good

Another example is the process of building software. I'm referring to the build stage here. I have noticed how sometimes the build in the developer machines are too slow. Slow builds in the local machine can be pretty frustrating for me. I have noticed caching come to the rescue in such cases.

Yet another example is - a slow Continuous Integration pipeline. The definition of slow could be defined by the team, depending on the context of the software. A slow pipeline has frustrated me at times

I believe that some popular ideologies in the industry help when it comes to a good developer experience. Let's look at some of them

Faster feedback when there are issues. I have always noticed that it's good to see the errors / issues as soon as possible. For example, the best thing I love about the modern Integrated Development Environments (IDEs) is that they show errors immediately while typing the code. I don't have to manually run compile or run any build command to see errors. Every character of code I type, there's a process behind the scenes that automatically checks the code for me and gives me warnings and errors. These can be lint warnings / errors or even programming language warnings and errors coming from a compiler / interpreter. The industry also calls this as a Faster Feedback loop, or cycle.

Shift left. In a sequence of processes in software development, when jotted down from left to right in a sequence, it's good to find issues sooner than later. So, the goal is to try and find issues more on the left or to do things to shift and find issues on the left than on the right which is more later and which means issues are found very late. This is just another way of saying faster feedback loop but at a more bigger picture level. For example, we do analysis and design, and then develop, and then test and finally release. It's easier to find as much issues as possible in analysis and design instead of letting them leak into the next stages and then find issues during development, testing or after releasing - by end users creating support tickets.

Simple ways to read code and debug code to fix issues. Debugging process should be easy. Reading code - it should be readable code without too much complexity, like it shouldn't have so much cognitive overhead - lot of thinking to understand the code?

Development tools to enable and speed up Development for the Developer. For example IDEs to help enable refactoring, writing code, changing code.
