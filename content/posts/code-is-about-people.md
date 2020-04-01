---
title: "Code Is About People"
date: 2020-04-01T08:48:01+05:30
---

Actually, everything is about people. Including code. I remember reading about
this in Twitter. Since then and up until now, I have seen multiple instances
where I can clearly say everything is about people, including code. I mean, I
guess we have made ourselves the centre of the Earth 😅 If we should be
inclusive, I think the phrasing should be - everything is about living
organisms. For this article, I'll limit myself to talking about just code. And
there might software for dogs, cats, fishes and many other living organisms, but
I haven't used them or seen them or have the knowledge to talk about them. So,
I'll limit myself to code for people due to the limitation of my knowledge 😅

Now, back to code :p If you think code is about people, I think you are already
onboard the ship, so you don't have to necessarily continue reading this. But if
you think code is NOT about people, do read on if you are interested to see some
small and simple examples to show code is about people

First, when we build software, what questions do we ask? "Why do we even need
this?". Usually there's some end user for the software who benefits from the
software. Browsers - who uses browsers? Aliens? May be. Who else? Humans.
People. We use browsers. Among us humans, each person is unique. We still do
have some commonalities between us, for example some of us work in the IT
industry. Software is usually written according to it's users. For example -
there are browsers and browser features specifically for developers - why? To
help developers do web development. To help make their lives easier - their work
and daily tasks easier. A lot of software you write - you would consider the
person using the software at the core of everything. If your expected end user
cannot use or understand your software, it's as good as a situation where your
software never existed. That's why User Interfaces and User Experiences are so
important! And every time you write software - you don't just jump and write
something that comes into your mind - you first understand the problem at hand,
a problem that your end user is facing, understand the end user who's going to
use the software, understand what features the software would need to provide
and in what way, to solve the end user's problem. You gather requirements, talk
to different kinds of end users, as not everyone is the same, and understand
them to understand how to make a software that suits them. A lot of times I have
made software - and failed, I think it's usually when I have failed the user.
Our software's success is based on the user's success. Now that we have seen how
software is about people in terms of end users, I want to talk about how
developers are also end users - for software like - development tools,
programming languages and it's ecosystems, software development processes,
practices, libraries and frameworks. Tools need to make the life of a developer
easier - if you choose a tool, how much ever famous or great people talk awesome
things about it, if it doesn't work out for you, may be rethink on it - you
could put extra effort on understanding the tool better with the docs, try to
make it work for you and stuff, but if you don't see value in it or if it feels
like retro fitting the tool which isn't suitable for your use case, may be it's
not for you, in which case you might want to reconsider. And if you are a
developer, every time you code, I would recommend to try to think about how you
can make your life easier when you do your day to day tasks - writing code,
writing tests, running tests, compiling / building, checking build pipeline,
deploying software, whatever it may be. If it doesn't look right to you, it just
isn't right for you and it's very possible that you can do better. For example,
if your pipeline takes a lot of time to run - check if it can speed it up - but
yeah, it depends, the effort to do that, possible cons like if you say
parallelizing is a good solution (it has it's own edge cases) and the value you
get out of it. But that's just an example. Another example could be - the time
it takes for your deployment. Another example - the time it takes for you to
debug an issue and find the root cause - you would want to make your life easier
to debug things easily so that you can easily go fix it (after writing tests
:P). Let me give an example from the open source world. I once proposed to use
Bazel tool in Helm repository. Here's the proposal -
https://github.com/helm/helm/issues/6557 . You can read the comments, the gist
is this - the proposal didn't go through. And I'm kind of okay with that,
because of the reason. The reason is this - it will be a bit of a learning curve
for someone to contribute to Helm if Bazel is introduced as the build too, instead
of using the plain go binary to do build. You see how people are at the core?
The maintainers don't want to add more tooling to burden a first time
contributor - a developer - a person, which is the same as making sure their
lives are easier while contributing. More examples are - everyone says - write
tests, write readable code, write maintainable code, have monitoring for your
application, write robust and performant software etc. Why do you think people
say that? A developer - a person, is going to read your program, to change it
in the future for feature additions and bug fixes, so your code needs to be
readable, and it should be maintainable - with tests, or else a new developer
contributing to the codebase will be scared of breaking existing features
unintentionally and they wouldn't even be able to find out as there are not
much tests or worse is no tests. To be able to maintain means - to be able to
change code for features and bug fixes. Monitoring for your application is very
important - so that you can know about the issues in your app before your
customer reports to the customer support - it's a fail fast mechanism. Or else
the time it takes for the customer to notice the issue, report it to customer
support and when multiple customers report, customer support will create a
high priority ticket / request to the development team and then you get to know
about issues and then you fix it - which is a big loop and takes a lot of time,
and all this time the customer - a person - is probably pissed off. And the
business is going to be really mad at the development team for being slow at
fixing the issue and you would be under pressure to fix it soon, given it was
detected very very late. Now, let's also look at this from another perspective.
Imagine you not being able to get an Uber for your late night ride to your
house - how annoying, right? Uber is software too, and you are a customer for
it. So, understand the pain points and see how you can fix them :) Same goes
for robust and performant software - your Uber taking forever long to book a
ride, or your ride getting booked and cancelled automatically and
repeatedly - how annoying is that? Robust and performant software is also key.
So, instead of looking at such things in software and software development as
is - look at them from the perspective of a person or from the perspectives of
different people, see how existing solutions or new solutions can help solve
the problems of people for a better life :)

I think, everything we developers do - following practices, doing software
development, using tools, software, people are at the core of things. So, if you
are doing something in your day job and it doesn't make sense, understand why it
doesn't make sense, for example, if you are following some practice and it
doesn't make sense - see if you need to change the way you do things, check how
you follow the practice and check if you are doing it right. If you are doing it
right, and it still doesn't make sense, then may be check if the practice really
gives you value, and what value it's intended to give and check if you really
need it - may be you don't need the practice at all and may be need some other
alternative or different practice for your use case or none 🤷‍♂️ if you don't have
the problem the practice is trying to solve. The same applies to tooling,
software and more.

The same analogy applies to all people irrespective of their jobs. We don't do
things (anything) for aliens I guess (or may be we do and we just don't
know? 🤔). We do things for people, for others, for ourselves, and many loving
people also do things for other living organisms - plants, trees, fish, animals
and more.

Let's keep living organisms at the core of what we do.
