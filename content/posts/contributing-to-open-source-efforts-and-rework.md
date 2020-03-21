---
title: "Contributing to Open Source: Efforts and Rework"
date: 2020-03-21T16:24:08+05:30
---

Sometimes, the craziest thing for me is - the amount of effort I put into
something, some work, but how I do it all wrong because I don't know some of the
expectations of the work. It's very crazy. 

In terms of Open Source, it has happened quite a lot of times for me I think.
I'll be working on a Pull Request to fix an issue. I'll take an approach and
then maintainers feel that the approach can be changed totally. One could say
it's a good experience, but still, all that work and effort being redone - and
the value of the old work is only the experience. Usually it's not too much new
knowledge, just an experience and it won't help if I keep getting the same
experience and I would be wasting lots of time if I keep doing it. I'll tell
what I thought about, to fix this issue. Let me tell you more such problems that
I have faced and some of my perspectives. Another Pull Request that I worked on,
unfortunately I spent a lot of time on a few tests in it. Again, you could say
that it's a problem with approach (kinda?) but in terms of testing where the
maintainer felt that unit tests are not great for testing interactions between
components and that some of those kind of tests I wrote, were not needed. Kind
of made sense. I need to learn more about testing. Anyways. So that was another
example. One more is - I missed checking for existing PR for an issue and raised
a PR, which became a duplicate. And then I found out later and maintainer told
about it too, and I had to just close the PR. I have also had cases where my PR
was superseded by another PR which was raised later than mine, reason being
there was some issue with my PR and I took some time to make some change it.
Anyways, it didn't get merged, it got closed. One good part about it was I
didn't spend too much time on it. It was a very simple and basic PR, more like a
first time thing for me. But yeah, I did spend time on it, even though it was
little.

So, what did I do about these? The first thing I would like to clarify is - I
have made a lot of mistakes, I still make a lot of mistakes, unfortunately some of em
repeatedly. I still have to work on and still am working on how to not repeat my
mistakes. Let me tell what I thought about the issue about maintainers asking to
implement a different approach in PR - I realized that the PR is gonna get
merged only if the maintainers are happy with the PR code. I thought I could
first discuss the approach with the maintainers first and then implement things.
It's not easy, getting the time of the maintainers. Sometimes they just say -
implement something and we can discuss - it's okay if it's something small. But
if it's a bit big, it's okay if you don't discuss, at least say it out loud
about the approach and any doubts and clarifications you have. Especially based
on previous experiences on PRs where maintainers didn't agree to something or
had some tid bits and thoughts on how some things need to be done. So yeah,
discussion or at least some form of communication before you putting in all that
effort - is a bit better. One thing to note is - I also have this thought - I
keep saying these, like rework and redoing things again can be a waste of time
sometimes, but sometimes I feel the reality is that - code in a codebase keeps
changing. People keep evolving it. My code could get rewritten any day. Idk. But
for the time being, it's good enough. But for that good enough state, may be we
could try to be productive about how much time we spend on it and not waste our
time. Time is key, can't waste it 😅 Anyways, back to the thoughts I have for
the other problems. About the PR with the tests issue and other kinds of things
where the maintainer had different thoughts and perspectives than me and
wanted me to get rid of some tests. I understand him, but one step I'm taking to
make sure I don't make the same mistake again for the next PR to the same repo
is - waiting for the PR I spoke about get merged. I want to see it get merged,
and then follow a similar approach and consider his ideas and do things for a
similar issue in another PR. But I'm gonna wait till this PR gets merged. Don't
wanna make a few more mistakes in current PR, that are not yet caught (since PR
is not merged), again in the new PR. So, yeah, waiting is the plan there. For
the other PRs, regarding superseding PRs that get merged - there are many
reasons a PR could be superseding other PRs. For me, the aim would be to - avoid
duplicate PRs by checking if another PR is already there. This is like a very
basic rule but yeah, I missed it, again, even though I knew it! 🙈😅 I have also
had other kinds of PRs supersede mine, especially PRs by maintainers. This was primarily
because the issue was a burning issue and the maintainer wanted to fix the issue
themselves and I guess they didn't want to spend too much time on giving reviews
and my approach was also completely different than what they wanted, so yeah, it
would have been almost a rewrite of the PR. And then, another place where my PR
got superseded was - a simple PR but due to some small simple issues, it got
closed and someone else did it right in their's so that got merged - solution?
I'm not gonna work on very easy PRs! One good thing that comes out of that is -
other first timers get to contribute to the repo, if I have already done some
easy first time issues. Or else I would always be grabbing easy things
and there will be nothing left for first timers. Also, another good thing is -
people will not be copying or fixing the same issue as me if it was some big
code fix or feature, compared to a small one. Big ones are a lot of effort. And
I would expect folks to not redo the work or copy work, intentionally or
unintentionally, as they could instead work on something else, something new,
unless I abandon the PR 🙈🙈 which I try not to. But yeah, it's a still a
problem in open source. Communications and awareness and it's also a problem of
decentralisation - there's no one to direct you, like a central authority, which
makes sure or helps you raise PRs only for issues that don't already have PRs.
I'm not saying we need a central authority 🙈😅 that would be really bad. But
the tooling should help - GitHub, or whatever platform we are on. Maintainers
also help with this kind of stuff - stopping duplicate work, but it's hard.
GitHub is actually helping with this a bit now - people can now see how many
PRs are present, related to an issue, when you check the issues list page. Previously
you had to find that by going into each issue page and scrolling and finding if
there are any open PRs for the issue.

Anyways, those were my thoughts on efforts and rework. Always try to make sure
that your effort doesn't go to waste. It's not easy, but try. :) Try your best!
