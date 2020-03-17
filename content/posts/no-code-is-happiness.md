---
title: "No Code Is Happiness"
date: 2020-03-16T19:46:29+05:30
---

Like, really, don't you think no code is awesome? Kelsey Hightower created this repo - 
https://github.com/kelseyhightower/nocode where he says there's no code, nothing to deploy, and that it
is the best way to write secure and reliable applications

Wow right? :P I mean, if you are a software engineer, and you get paid for writing no code, that would be
awesome right? 😂🤣 Well, not gonna happen I guess. You Will write code. You Will deploy it if it's some
kind of service, and You Will maintain it. But you can still apply some "principles of no code" in your
software I guess. "How?" you ask? Well, for starters, fall in love with deleting code, don't fall in love
with adding too many features, especially unnecessary features. Why? Let me give an example first. Have you
heard all those statements that people tell that they love it when they remove code? That they love it when
their git commits contribute more to deletions than additions? They are well experienced. They understand
what it means to remove code. Removing code means less code. Less code means less things to be read, less
things to be understood, which probably means less complexity - who doesn't want simple and boring code? And
simplicity is key for more people to contribute to the code! Less code means less possibility for bugs -
bugs cannot exist if code doesn't exist :p But hey, don't over do this and write unreadable succinct code!
🙈😅Probably that's a blog post for another day 😂 

So, how can you follow these? Let's say you maintain an open source tool - a small tool that does something
- it has some core functionality. People come and say - it could do more and that it would be the tool's
responsibility to do it, that it's best to put that code in your tool. The reality is - it depends. On a
lot of factors. Let's say this feature is really something useful for a very small fraction of users of the
tool - but that's just a small fraction. You don't bring in extra features - meaning extra code - to
satisfy a very small fraction of your users and incur the cost of 
1. Maintaining the feature - as features come with possible venues for bugs, and the possible demand for
more features on top of the new ones
2. Possibly increasing the complexity of the software and complexity of the core too. And if it's complex, 
it will be hard to debug, reason out, and fix. In turn affecting all your users that depend on the core of
the software but not on the new feature used by just a few users.

You need to be really careful about what features you let in to your software. So this is a place where you 
can stop extra code from even coming into the picture - so that you still have less code.

And if your users are stubborn about new features, which they usually are :P one possible good solution I 
can think of for the problem of feature demands by users is - extensibility of the core and an extension or 
plugin system. I'll just call it plugin system for simplicity. Make your core software extensible through
a plugin system. I'm sure you have used plugins if you think you haven't used it or heard of it. Browsers
have plugins - ad blocker, dark mode, and what not. Firefox and Chrome browser call them as extensions. And
there are more examples for extensions, like HTTP services providing webhooks for integrations - example 
slack incoming webhooks by slack service. May be this is a blog post by itself for another day ;) But you
may ask - How is this a solution to users demanding features - well, you can tell your users to implement
any extra features as an integration to the core of your software, or someone else could build it too. It's
just that you don't have to, for now at least, and hence you don't have to maintain it. This way, your core
software remains neat and clean and small with less code, and people can extend it to their will based on
the extensibility of the core. 

Another place where you can aim for less code is - focusing on the core features of your software. If your 
software does a lot of things and may be even reinvents the wheel - implements something that a lot of 
mature libraries already provide, I would recommend delegating whatever custom implementation your software 
does, to the library. So remove all of that custom implementation you have done. Let the library do it's 
magic :) And if you are someone who thinks your code should have 0 dependencies or as little dependencies 
as possible, remember that - you are unnecessarily reinventing the wheel instead of delegating. You can 
ignore libraries if you think you can do a better job than them and hence the custom implementation, or if 
you wanna just give it a try. It all depends on what's the core functionality of your software and why you 
do or don't want to use libraries. I would recommend using libraries. You could say that libraries have 
bugs too - Yes. Agreed. Well, you can go contribute and help them. They have figured out almost everything, 
other than some small bugs. Bugs are okay. Softwares have bugs. If they spent years on the library, you 
should really think about writing the same thing again and be sure you really wanna do it. Or, you could 
just use all those years of maturity of their software based on the years of experience of the developers 
who worked on it and all the users who use it and have used it and their feedbacks and reported issues
People usually call this as - standing on the shoulders of giants. So, I'm recommending you to stand on the
shoulders of mature software and libraries, and just focus on the core of your software. If you want to 
start a small business, you wouldn't start from scratch - reinventing the computer again with new
components, reinventing a programming language, reinventing a framework and then using it in your business
So, yeah, use all those years of experience, maturity and the learnings from failures, instead of maintaing
more code. You could instead spend that time on your software's core, or on a new software or in your life -
work life balance :)

That's all I had. If I think of more ways to reduce code, I'll put that in a part 2 I guess :)
