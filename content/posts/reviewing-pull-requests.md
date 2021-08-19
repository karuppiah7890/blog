---
title: "Reviewing Pull Requests"
date: 2021-08-19T20:07:30+05:30
---

I haven't reviewed so many Pull Requests (PRs) to be writing about them. But reading some of the reviews for my PRs and also others PRs and also having done a handful, I have come to realize a few simple things for reviewing PRs. So let's get right into and see what they are! :)

First off, I want to call out that every tip / advice / "best practice" / idea is based on a particular context. In this case, the context I want to provide is - what my experience is - my experience with PRs is mostly around open source and also a few internal closed source projects / yet-to-be-open-sourced projects. I have worked with GitHub and GitLab a lot and a bit of BitBucket too. But I have tried Pull Requests (PRs) / Merge Requests (MRs) feature mostly in GitHub and GitLab only, never once in BitBucket. My experience is completely around Git. Now, with that context, you would have a good idea of what I'm talking, so let's get started! :) And remember, only you know what works best for you, and only you know your context best, so apply your context and ideas and tweak accordingly rather than taking my words as is! :)

# Acknowledge

So, first things first, when someone raises a PR, and if it's an open source project and if it's their first PR or probably second or third, meaning they are still a new contributor, just thank them for their effort, and maybe even congratulate them, depending on whatever is your style of communication and interaction. This is quite important in open source projects for newbies as they might have put a lot of effort if they are pretty new to open source, GitHub / GitLab etc, Git etc, concept of open source, concept of PRs. So, any sort of acknowledgement to their massive efforts is a great thing, it would probably make them feel good

Acknowledgement is key, as it shows that you have seen it - the PR someone raised, the issue someone raised. When you show acknowledgement in some way, you show that you care. This is kind of like a human touch to the PR, to show that you care. Atleast that's my perspective. If I raise a PR and no one seems to even acknowledge after days, weeks and months, I might as well close it at some point, simply out of frustration. But yeah, some projects might be lacking contributors, maintainers, or there might be some burning issues / more priority PRs, and again, because of lack of people, there might not be enough people to look at all the PRs. But in my perspective, simply acknowledging and saying "Hey, thanks! We will take a look at this PR when we get time!" is still a cool thing! In some of the bigger open source projects, bots do this with the help of bot automation and backend systems, for example GitHub bots comment on GitHub with the help of bot accounts. This may or may not resonate with all, but the bot is usually helpful to give some important information, for example, asking the contributor to ping someone to review the PR, or to assign to the PR to review it etc

So, that would be my first thing, to show that I have noticed the PR, acknowledge it. I would apply the same to issues too, when issues are raised. Not to mention, I have been guilty of not acknowledging PRs / issues in the past, but still, this is my hard learning based on experience.

# Ensure Checks are good - Tests, Lint checks

It's really important to have some good code in the PR so that the final codebase after merge is all good. Although long ago I have seen some ideas about "Optimistic Merges" where people just merge optimistically and make changes later, I currently see most people having some sort of "gates" to gatekeep the quality of the PRs. People generally call it as quality checks, or quality gates, in the world of software quality, where software / code has to go through many quality "gates" to be qualified as a good software / good code

Some of these gates are -
- Tests - automated tests like unit tests, integration tests, functional tests, end to end (E2E) tests etc. There are so many automated tests out there. Note that some people are very particular about naming of these tests rather than the concept of testing itself, beware of them ;) Others who care about testing and not it's name, here's a surprise, in my experience, I have noticed some ambiguous names for some kinds of testing / alternative names for the same thing, and some people extending the idea of some testing techniques, which I think makes sense as long as everything works and software is tested. There are also unpopular opinions around unit tests and how integration tests are better, and also about how to test best, with less change to tests when code changes, kind of like less coupling between code and tests / test code. In any case, tests are quite popular and many projects have them, and many projects run the tests using Continuous Integration (CI) pipelines in an automated manner for PRs and commits and software releases and what not. So, when someone raises a PR, simply check if the CI status for tests is green - which means all tests passed! :) Or else you could ask the contributor to check the test failure if the CI status is red and in a failed state

- Lint checks - There are many kinds of linters out there, which lint different kinds of files and code, to show errors, warnings and also formatting / styling errors and warnings. If the project has lint checks and runs them in CI pipelines, do check those too in the CI and check if the status is green, or else just ping the contributor to take a look at the red CI pipeline

There can be many other gates too. For example, if it's a compiled language or if there is a compilation / transpilation step, before tests are run, first build / compile is checked, again in a CI pipeline usually, for automated testing and checking. If some of those fail, again ping the contributor to fix that

# Look for other problems in the code. Automate review when possible

Look for problems in the code - like typos, logic issues, dead code etc. When you review the code, think about how you can automate the review in the process, especially for simple things. For example, typos can be detected and also automatically fixed by some linters. You could use the linter in the CI pipeline to detect typo and then ask contributor to use the same linter in local to fix the typo and push the latest changes with the fix. Same applies to other kinds of review comments too. For example, checking if license header is present in each of the code files, this can also be done automatically by some linters

Other kinds of issue that you could look for is - signing commits, signing Contributor License Agreements (CLAs) etc. All these could be checked automatically and some bot could reply with comments if it fails, and it could provide some help on how to fix the issue, that would help the contributor! :)

Another popular thing I have noticed with newbie contributors is that, even with all the `Signed-off by:` in the Git commit message, sometimes you can see that the Git commit username and email are configured wrong or are the default, based on the computer configuration. This can lead to a not so great experience for the contributor. Why? because when people contribute on platforms like GitHub, GitLab etc the platform identifies people with the email ID and name is just an extra thing, but if the Git commit does not have the correct name and email ID, then the platform will not show that the commit was done by this particular user. So it's important to help contributors connect their email ID to their Git platform account and also use the same email ID in the Git commits for author field, so that the plaform knows who contributed and will show up in the contributor's history of contributions which is a pretty valuable thing for contributors, which they may not know initially. I think this is a pretty important thing. I have seen many newbies make a mistake here by using company email IDs, or just wrong email IDs etc, basically the wrong email ID

# Call out nitty gritty stuff

When finding very very small issues, like typos and similar, it's good to call out that some of them are nitty gritty stuff / nitty gritty details, to acknowledge that everything else looks good and that the review comments are just to fix some extremely small things, usually for some small / simple reasons or to just make the PR perfect, instead of creating more PRs later to make things more perfect with extremely small changes

# Check commits

It's important to check commits - commit messages and the sizing of the commits and also the commit content of course, to understand how the commits have been done / split, in case there is more than one commit. Depending on the project, the Git commits will be required to follow some rules for consistency and for ease of use - to read and understand Git commit messages in the future. So just check if that's followed! :)

# Conclusion

That's all I had for this post today. If I come up with more ideas for reviewing PRs, I'll create a part 2 ! ;)
