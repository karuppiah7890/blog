---
title: "Learnings"
date: 2021-05-07T14:05:35+05:30
draft: true
---

I have been working for three years in ThoughtWorks now.

I still feel like I don't know anything. I think it's mostly my fault, my
tendency to not get into details of stuff. I usually tend to work in an
abstracted manner and not get into too much details. I sometimes feel that
there's lot of complexity, and that some of them is just really needed to be
understood if it doesn't concern my daily work really.

For example, I have noticed people talk a lot about git tool and tons about it
and about it's internals and the tons of features and how to get out of problems
if any - and how it's so easy to mess up and people write comics and what not
to share the remedies in case people make any mistakes.

I think it's all great - but I really don't use all of git's features that much
really. I use it in a very basic manner - I use it to stage files, commit it,
I always "git pull --rebase". I need to learn about fetch and rebase more. I'm
usually not doing "git rebase", or even working with multiple branches, at least
not in my day job. I very rarely use branches - as here in ThoughtWorks, we
mostly follow trunk based development whenever possible, and use branches in
rare cases - like when we have to push some temporary code to share it with
a team mate since it's not complete and half done, so cannot be pushed to the
main branch. But yeah, I do use branches in open source and forks. It usually
boils down to exactly the below

```bash
$ # short form of - git checkout -b <branch-name>
$ gco -b <branch-name>
$ # when I need to rebase with main branch
$ g rebase <main-branch>
$ # resolve conflicts and use the commands mentioned by git to continue,
$ # for example
$ git rebase --continue
```

I also know some more stuff, like tags, and difference between tag and
branch. And some more basic stuff, like revert, cherry pick. But many of them
are rarely used.

That's me and given this, my friends just overwhelm me with details like how
commits are stored and what not. I mean, I get it, it's nice to know internals
sometimes. I feel like I'm a genius :P It feels good, but that's all it is. It
does help to understand how a software was built to learn it's architecture and
appreciate it and appreciate it's thought process and even apply it's
principles if possible in other problems that we encounter. It's just that I
just get overwhelmed, so I still haven't done stuff related to ref log and what
not. Thankfully I have not lost and messed up anytime, except for once, where
I checked out the contents of some file by mistake. Forunately it was a small
peice of code. 🙈 Anyways, now, back to what I was saying, yes, I usually tend
to just scrape over the surface. I have also found that I have to spend a lot of
time on going over the details and let's be honest - there's always lots and lots
of details.

I think one good thing is - I read somewhere that it's good to know some depth
but a good amount of breadth too. Knowing everything is just going to impossible
is what I feel and have read too. It also gives me FOMO - Fear of missing out.
Anyways, that's that. Also, there's this thing that I read - usually anything
that you learn in tech will become old after a few years. I think now it's a
few months or weeks. I mean, something new is coming everyday. Some new features
are coming out everyday. Sometimes it feels like people are making software more
and more complex to use - and it's funny how people ask us to get into the details
and learn - I mean - why? Why do it? You know one thing that it helps in? Knowing
the details - when things go wrong, you know where to look and what to look for
and how to fix it maybe or at least how to approach the problem. I would question
back by asking - what the hell has software become? Why is it so bad? I mean,
in all these years, I have never trusted software usually. I mean, no wonder
now I understand the importance of tests and how I have come to learn from
experience in open source that tests are so important, even though my team mates
have told me a ton of times that it helps to give confidence about the software.
Our softwares are so bad that we need to know how they work. The other good things
about knowing details or under the hood thing is - you learn, you learn a lot.
So much so that you can even create something yourself, or maybe contribute to
it and you will also be able to solutionize better if you know how to use your
tools to the best manner - for which you need to know your tools very well to
know it's capabilities and options and features and what not, so that you have
the most information to make a calculated decision on which of the solutions to
choose from when you have multiple options.

I have worked in very few teams till now. In college one project as a freelancing
project. College projects were a funny thing, it wasn't really team work. And
then during company training one more team. And then one team in an internal
project and then 3 client projects. Totally 6 teams. And also working in teams
for stuff like events, and workshops and what not. They were all short lived.

I have learned quite a bit about myself in this journey. I have clearly noticed
myself being some sort of rebel - some sort of an outsider, always finding
problems, mistakes. I don't feel great about it. In fact I acted so badly in
my internal project in my company. I'm still wondering how I managed to do that.
I think it was because I was going through something at that point. I still
can't use that as an excuse. I guess age and experience has taught me some
things. I remember how I used to blame people, pin point at mistakes, always
being the pessimist. I remember how someone told me that I'm also part of the
team when I was accusing them of mistakes. And how I should be part of the
efforts of doing some work regarding fixing issues in the work that we were
doing. I think I have noticed a lot of this behavior in me a lot of times in a
lot of teams. I realized that sometimes my opinions just come off too harsh or
bad. I still haven't exactly learned to voice out difference in opinions, in a
nice way.

The only good thing about this whole thing is - someone once complimented me on
how I can remove myself from the equation and look at things and comment on it,
when I sprung like a damn rubberband between two fingers. It hurt me a lot. I
hate it when I get angry and mess things up. But this person, a colleague, she
told me that it's not an easy thing to get out of the equation to evaluate
things. I was even given some book references to read. I still haven't checked
them out. My bad! 🙈


