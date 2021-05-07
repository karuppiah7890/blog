---
title: "Developer Experience Part 1"
date: 2021-05-07T20:59:45+05:30
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

- Faster feedback when there are issues. I have always noticed that it's good to see the errors / issues as soon as possible. For example, the best thing I love about the modern Integrated Development Environments (IDEs) is that they show errors immediately while typing the code. I don't have to manually run compile or run any build command to see errors. Every character of code I type, there's a process behind the scenes that automatically checks the code for me and gives me warnings and errors. These can be lint warnings / errors or even programming language warnings and errors coming from a compiler / interpreter. The industry also calls this as a Faster Feedback loop, or cycle.

- Shift left. In a sequence of processes in software development, it's good to find issues sooner than later. Now let's say you jot down these processes from left to right in a sequence. It's kind of like a timeline of the processes. So, the goal is to try and find issues more on the left or to do things to shift and find issues on the left than on the right. As being more on the right means it is more later and it means issues are found very late. This is just another way of saying faster feedback loop but at a more bigger picture level. For example, let's say we do analysis and design, and then develop, and then test and finally release / deploy. It's easier to find as much issues as possible in analysis and design instead of letting them leak into the next stages and then find issues during development, testing or after releasing - by end users creating support tickets.

![sequence-of-processes](/blog/img/developer-experience-part-1/sequence-of-processes.svg "sequence-of-processes")

These are just some examples that I can think of as of now. But these are pretty good. Let me give an example of how this helps at a developer experience level and also at a bigger picture level.

When having faster feedback loops / shifting left, the team as a whole find issues faster and rectifies / corrects them faster. Same goes for developer experience. What happens when you find issues later and not sooner?

Let me give you an example. In my previous project, whenever we had to push the code to the git remote repository, we would run a sequences of commands manually or automatically using git pre-push hooks, these commands would basically run a lot of checks that are also run in our Continuous Integration pipeline. What are these checks? It would be things like - run tests, run compile, run linter etc. Each team has it's own set of checks to ensure that the software quality is great! Now, if we don't do run these checks in our local, the code would be pushed and it would fail in the pipeline and pipeline would be red. In such a case, people pulling the latest code from the git remote repository would have a dirty / not-so-proper code as it's failing in the pipeline. This is not a good state to be in. So, it's important to find issues in the local as much as possible before pushing the code and letting it fail in the pipeline. Why? Because others pulling the code from the git remote repository or any repository would expect a clean build which passes all the checks in the pipeline so that they can make changes and be sure that only their changes can fail the checks if at all the checks fail. Also, the other thing is - when you don't run checks in the local, you have to wait to push the code, see the pipeline run and then see it fail there. That's kind of slower. Instead you can run the same checks in your local and it will be faster and it will also not affect the work of your team mates as much as possible.

Some might say - why run the checks in local and in pipeline. The pipeline is meant to help with issues like "works on my machine" and also helps to check if the code works in a completely new sandbox environment every time. The pipeline also ensures that all the checks are run even if the developer does not run the checks.

So, we had to run checks in our local. If we didn't, it would just be a bad developer experience for almost the whole team. Now, while running checks in our local, that can have bad developer experiences too. Let me give you an example. Now, in our project, we had to run tests, compile, run linter. We ran these in a sequence and not in parallel, in exactly that order. Tests and compilation took a lot of time actually, and linters were pretty fast. In my case, I used to use the automatic pre-push git hook to run these. The craziest part was - the tests and compilation would take so long and complete and then I would get a linter error. I would have to fix it and run all of them again most of the time, or just run the linter if I'm so sure that the compile or test won't be affected by the fix and then push it with no pre-push hooks. An easier alternative would be to run the linter first, which runs fast and gives errors / issues faster, and then run the test and compile which take more time to run. This way, I find any possible issues way sooner. At least that's what I prefer!

Other good Developer Experiences that I can think of are

- Simple ways to read code and debug code to fix issues
    - Debugging process should be easy
    - Reading code - it should be readable code without too much complexity, like it shouldn't have a lot of cognitive overhead, which basically implies that reading and understanding the code requires a lot of thinking

- Development tools to enable and speed up Development for the Developer. For example - powerful IDEs to help enable refactoring, writing code, changing code

## Conclusion

Developer Experience is really a very relative term. Each developer might prefer some things and expect some behaviors from the systems they work with and would love some kind of experiences. It's very important for the team to come together and talk about what's going well and what are the pain points and how it can be improved in a meeting. Retrospective is a popular name for such meetings.
