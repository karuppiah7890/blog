---
title: "OSS Stories Part 1: Write Tests to Fix That Bug!"
date: 2020-03-08T15:10:08+05:30
---

I recently thought I should share some of my Open Source Stories! Stories about how I contributed,
what happened, some learnings, swags and what not. Anything that happened with respect to Open
Source for me. I think I'll try to write as much stories as possible! Not all of them will have
technical stuff. It's a story :P It could have anything. So, today I have one story to tell ;)

This is something that happened when I was contributing to [Helm](https://github.com/helm/helm). I actually 
still have some contributions to make 🙈 Let me finish some blog posts before that 😅😂

So, [mumoshu](https://github.com/mumoshu), a humble developer, raised an issue - 
https://github.com/helm/helm/issues/6788 mentioning how a 
[bug he reported](https://github.com/helm/helm/issues/6384) and
[fixed](https://github.com/helm/helm/pull/6385) was broken again by
[another PR](https://github.com/helm/helm/pull/6341). Now, I'm the type of guy who tries to never have 
blame games as they don't really help. I do rant by myself, but find my way to work and fix issues. And
I also like to put tools and processes and automation in place to make sure issues don't occur. In this 
case, I'm sure it wasn't the intention of [this PR](https://github.com/helm/helm/pull/6341) to break the 
other [PR](https://github.com/helm/helm/pull/6385)'s fix. But how would we even know that if some PR / code 
broke some functionality or feature or expected behavior? Through tests, right? So, that's what I requested
[mumoshu](https://github.com/mumoshu). To write tests for it. Initially he mentioned that it would not be 
easy and that he wanted to keep fixing it when it broke. I recommended to try and get help and fix it, and I
couldn't really help because I had never worked on that part of the code - helm plugins, and I had other
issues and PRs to check so I was caught up with those. But finally, [mumoshu](https://github.com/mumoshu) 
did it! He wrote the tests for the issue and fixed the issue too! So, the next time someone breaks it, we
will automatically know from the failing pipeline :) In fact the maintainers were very happy with the
tests. This was the PR [mumoshu](https://github.com/mumoshu) raised - 
https://github.com/helm/helm/pull/6790 and some comments praising the tests - 
[this one](https://github.com/helm/helm/pull/6790#pullrequestreview-308004834) and
[another one](https://github.com/helm/helm/pull/6790#pullrequestreview-308052291).

So, finally it turned out to be a happy ending as usualy ;) A thing I learned from this is - always write
tests for your bug fixes, so that later when somone reintroduces that bug, the tests will fail. So, this is
what I do when I work on bug fixes

1. Write a test for the expected feature where the bug is present. Since the bug is still present, the test
I'm writing should fail
2. Use debugger, find the bug, fix the bug
3. Run the tests that I wrote to see if it passes - it must as the bug has been fixed now

In other words, Test Driven Development - write a failing test and then write minimal code to pass it

That's all I had for this story. Till next time 👋😄
