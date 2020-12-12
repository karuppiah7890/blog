---
title: "Dead Code is Dead! :P"
date: 2020-12-12T14:41:35+05:30
---

## TLDR

Dead code is just unnecessary code that's lying around. You don't need it,
nobody needs it. Just remove it and make your lives easier by removing
unnecessary things :)

If you want to take it up to the next level, use automated tools to detect some
kinds of dead code to make your lives more easier ;) Use the same automated
tools to fail Continuous Integration pipelines when dead code is detected.

Some kinds of dead code can only be found by developers probably. So, find them
and remove them!

If someone says they need the deleted code back, get your version control
system's (for example git) help. Go back to the old version and get the code
back.

Note: If you don't use a version control system, I would highly recommend using
one before deleting any code. :)

## Longer version

Recently I have been doing some small cleanup and tech tasks in some of our
project code. This happens whenever I see something that can be improved and
have the time to fix it or I at least add a TODO comment in the code, so that no
one in the team forgets it.

There are many different kinds of cleanup and tech tasks. For me personally, the
best ones are the ones where I get to delete code :D :D

I love deleting code :P I love removing code to have less code. This way

- We have less things to maintain. Every piece of code you write, you are going
  to have to maintain it. Which means, you have to improve it if needed, fix
  bugs. That's all good - but do you really want to maintain something that you
  don't even need / use? Why would someone do that? No one would, if they knew
  that the code is unused.

- We have less things to read and understand, less complexity in our lives.
  Every piece of code your team writes, all the people in the team are going to
  read it at some point and will try to understand it for various reasons - to
  simply understand the system they are building, to modify the code or reuse
  it, to fix a possible bug that they are still debugging. Now, this is all
  good when the code is needed. But if it's just unnecessary code that no one
  needs or uses, then why would someone spend their time reading it,
  understanding it?

When I say less code, of course, less code should still be readable 😅 It would
be counter productive to delete 10 lines of code and use 1 line which no one in
my team can understand and probably even the person who wrote it won't
understand after a few days or months.

So, clearly I don't like having code that's unnecessary - it just leads to
problems like I just described. I'm also a minimalist - do more with less, leave
out anything that's not needed.

Usually this kind of code is called unused code, or dead code or code that is
not executed or just unnecessary code which is not needed any more due to some
reason.

### How to detect dead code

Let's start with some examples of dead code!

Some simple examples of dead code are

- Commented out code. This is usually pretty straight forward. Surely comments
  are something that are not used or executed. People usually use comments to
  document or give some meta information about code. But commented out code is
  just usually confusing. I think it's just unnecessary to have commented out
  code.

- Unused variables. No one uses the variable. It just stays there in the code
  and people will read it, thinking it might be used somewhere. They will also
  try to understand it. But if it's unused, it's all a waste of effort to try
  to understand it.
- Unused methods and functions. No one uses the methods or functions. There can
  be various reasons for this. Usually, it's just unnecessary to have such
  unused methods and functions. Someone will read it thinking it's being used.
  They will try to understand it but that would be just a waste of effort again.
- Unused classes and structs. This one's pretty crazy. Usually classes and
  structs are big entities, so if it's unused and someone doesn't know, they are
  just going to spend a lot of time try to understand it and it would be a big
  waste of effort.
- Unused function parameters. This one's a tricky one. This is more about a
  function that's being used but some of it's parameters are unused in the function code. What this leads to is - people using the function just pass
  these extra parameters, assuming it needs it, but the function does nothing
  with it. So the callers of the function have to spend some time and effort to
  just pass this parameter which is not even used. It's just plain sad and waste
  of effort.

There are probably lot more examples! These are just a few.

How do you detect these? One of the ways would be - when you are coding, some
Integrated Development Environments (IDEs) show you warnings for some issues
when you are writing code. One pretty popular IDE warning is to show unused
code.

For example I use IntelliJ IDEA for Java development. IDEA shows me unused code
in grey color with a warning that no one is using it - meaning no one is
referring to the code (variable, method etc) that I'm seeing.

This is still a manual process, because you still need to use the IDE and "see"
that there's warning and then detect that there's unused code in your project.
This is usually useful when you are typing new code and that new code is not
used anywhere because you just wrote an alternative for it, so now you can
delete it by just visually seeing that no one else is using the code because it
has a grey or some color with the IDE's warning about unused code. This also
assumes that the IDE has the feature to show unused code using built in
features or using some IDE plugins. And you also need to help your team mates
to have the same features in the IDEs they use or ask them to use the same IDE
that you use.

One advantage of getting IDE's help is - it's a very fast feedback in you
development cycle. You get some information while you are writing any new code
and you can immediately remove the unused code if the IDE warns you about it.
So, I would recommend you to check for IDE features or plugins to help you find
unused code, if you don't already have these features.

Even if the IDE has such a feature, IDE can only do so much. Not all dead code
are easy to find.

This is mostly because some code are clearly unused, while others are unused in
a complicated manner. This is the reason why some dead code are easy to find
while others might be a bit obscure to find.

Let me give you some examples of hard to find dead code.

Let's say you have some code and some tests (test code) written for it. If the
code is only used by tests and nothing else, then it's still dead code 😅
Ideally the code has to be referred / used by some other source code for it to
be useful.

Maybe some IDEs can find such dead code too, warning you saying "hey, this code
is only used in tests"

Some IDEs can also find dead code like this

```javascript
let blah = 1;
blah = blah + 1;
```

Let's say those are the only two lines of code where `blah` has been used. Which
means that `blah` has been used only to update itself but not used anywhere
else, for example to just read the data alone and use it. So, `blah` variable
looks pretty much useless.

> blah variable : Ouch. That hurt.
> Me: I'm sorry...

Another example is, recently in our project, we decided that we don't need one
of the endpoints that we have in our microservice. This endpoint was mostly used
only for development purposes but we realized that we can manage things without
it. Even with that decision, we took some time to remove it. Since we took
some time, in this time people maintained it, fixed syntax errors, fixed failing
tests for the endpoint - just because it existed. If it didn't exist, people
wouldn't have had to do all that. So, when you take a decision to remove some
feature - remove all the code related to that feature to avoid spending effort
on maintaining an unused / unnecessary feature.

A few more examples of hard to find dead code

- Unused tools / frameworks / libraries / similar softwares in the project.
  Sometimes the team may decide to stop using some software. But it's possible
  that the code, configuration, dependencies related to this software will
  still exist in your code. This is just unnecessary stuff which you can simply
  remove.
- Unused dependencies in the project. Sometimes, while doing development, we
  might be experimenting and trying out many libraries. We add these libraries
  as dependencies in our project's build system and try them. It's possible that
  we might miss to remove the others once we decide on which library/libraries
  to use. Also, I have noticed that sometimes we might add some extra
  dependencies than what is needed. A good understanding of just what is enough
  will help in such cases. This key because having extra unnecessary
  dependencies gives other people an idea that "oh, these are the dependencies
  that we need for the project and tests to run." when in reality, it might
  not be the case
- Unused components in tests. Recently in our project, we noticed that we run
  integration tests with actual DB and Kafka running in Docker container. These
  are two components that our services use. In these tests, we turn off any
  Kafka related features or mock the Kafka related code. What this inherently
  means is that the tests would just run fine even if the Kafka was not
  running. But this is hard to find out until you really analyze the whole
  setup and understand the tests and the reasoning behind them. We also run
  Kafka integration tests separately in another project which is the Kafka
  client project. So, finally we removed the Kafka container and it made the
  integration tests run faster
- Unused features in code when running tests. This one's a bit tricky. Here
  it's not exactly unused code, so it does not exactly fit into the
  definition of "dead code". This is more about how test code
  tests only some parts of the code and other parts can just be mocked if
  possible. But sometimes tests don't mock the unused features in the code.
  This can be okay, or sometimes problematic too. It really depends. I'm going
  to cite a similar example as the previous one. In our project which uses
  Kafka, we had some integration tests where Kafka was not
  needed, but some of these integration tests didn't turn off Kafka features.
  These tests worked and passed, yes. But what this lead to was - we could see
  that the test logs showed that the code tried to connect to Kafka multiple
  times and failed after a time out error - as Kafka was not running and was
  not needed too, for the test. But because of this attempt to connect to Kafka
  multiple times, the tests ran for a lot of time and was too slow. In this
  case, an unused feature in code (Kafka code) ONLY in the test was causing
  issues - it could be avoided by using feature toggles in tests and turning
  off Kafka features or use mocks in tests. Again, this was specifically about
  test code and unused "features" though the code is certainly valid and is
  needed for production and for other source code.

#### Automation to detect dead code

We just spoke about different kinds of dead code you can find in your project.
We also saw how IDEs can help with detecting dead code - a few kinds at least.

Usually, some basic kinds of dead code can be easily found by many tools out
there. So, instead of you manually looking for dead code, tools can help you.

Some special category of tools usually have such features of finding dead code
and similar code quality issues. Such tools that help with code quality are
called Code Quality Tools. For example linters are a kind of code quality tool.

Manually checking for dead code can be very hard in any project including small
ones, and it's nearly impossible with big ones. Tools can do a better job with
finding some of the issues and you can sit back and relax :)

You can also integrate such Code Quality Tools in your Continuous Integration
(CI) pipeline, if you have one. I would highly recommend having a CI pipeline,
and failing the build/pipeline when there's unused code / dead code detected.

There are lot of Code Quality Tools out there for different languages. Also,
some languages have built in tools to help with this. For example Golang
compiler can help you with finding unused variables by showing compiler errors
for it ;) It's pretty slick!

If you are using JavaScript / TypeScript, you can checkout existing ESLint
rules which help with finding unused variables, methods etc.

I believe there are linters with such rules in other languages too :) If not,
look for specialized tools that look for just dead code. If you don't find any
for your language / tech stack, maybe build one? ;)

In general, I would recommend using Code Quality Tools to help you with
checking the quality of your code, apart from just finding dead code. Dead code
is probably just one metric for a good quality project. Having less or 0 dead
code is a good thing for a project - it means that the code quality is good!
There are many other metrics to inform you about the quality of your code.

### Removing Dead Code

Usually, presence of dead code would mean that you would have to delete the
dead code, or if it's an unused "feature" in test code causing issues, turn off
that feature or mock it - that was a very special case.

Removing code, dead code in this case, may sound pretty scary for many people
for multiple reasons.

For example, someone may think that "Someone else wrote this code. It was a lot
of effort. Why delete it. Also, what if they want that code back again later?"

Or there might be thoughts like "I'm not sure what this code does. Is this code
really needed?"

In such cases, I would recommend talking to the team first and try to understand
the need of such code that you have detected as dead code and if you think it
can be and has to be removed if it's unused, vouch for it :) Clarify with the
team and take actions accordingly.

Also, I have some warnings about deleting dead code when you are very sure
about the fact that it's dead code and don't plan to talk to the team as you
are too sure. I would still say inform the team at the least as people usually
don't like surprises :)

Warning - Understand the code and ensure that no one is using it. For example,
if the code is actually a library that's imported by others, you can never
easily find out who is using it and who is not using it. Especially public
external libraries, like Open Source libraries. Also remember that removal of
code is usually a breaking change - it breaks backwards compatibility of the
project. So ensure no one uses the code before removing it. That's one of the
main definitions or attributes of dead code 😅 "it's not used by anyone and it's
not needed"

#### Special cases where I have not removed dead code

In very rare cases, I've not removed some unused code. For example, we have end
to end tests written with Selenium and it has some helper functions. These
helper functions are like utils (utilities) that we all use to write lot of
tests. I once noticed that one or two of them are not used anymore. They were
dead code. I could have been brutal and removed them. But I noticed that they
were good utils and left it alone. It's kind of like against YAGNI (You aren't
gonna need it) principle, but I let that one slide. 😅

I guess now you must be like "what's this guy doing? He tells me to remove dead
code. Tells me all the bad qualities it has and then chooses not to remove some
dead code from his project due to some reason?"

While I'm writing, I'm actually tempted to rethink my decision and go ahead and
be brutal and remove that utility function which isn't used anymore :P

My only thought process was "If I remove this now, someone else will have to
learn and write a similar utility function later when it's needed. Or if I
remove this now, maybe someone will not be able to discover it and use it and
have a good test using a nice utility function". I was basically thinking about
the helpfulness of letting it stay there and also the effort of writing it
again.

But yeah, if I remove it, it's just a small utility function. Maybe someone
else will think to write it again if they need it :) It will be some effort
though, to learn some Selenium and write some lines of code :P But that's
inevitable in coding I guess :)

But yeah, talk with the team when you have second thoughts or are not sure about
the dead code and removal of it. It will help :)

### Funny / weird analogies

#### Hunter

When I'm looking for things to improve in the project, especially unnecessary
things that I can delete, I'm like some sort of hunter, looking to hunt down
something in the project and remove some code. :P

And when I find something to delete, that "Got you!" feeling is awesome ;)

#### Soul Reaper / Grim Reaper

It's like the developer is some sort of soul reaper / grim reaper getting rid
of the dead code (dead thing) from the project

![soul-reaper-or-grim-reaper](/blog/img/dead-code-is-dead/soul-reaper-or-grim-reaper.jpg "soul-reaper-or-grim-reaper")
[Background vector](https://www.freepik.com/vectors/background) created by [macrovector](https://www.freepik.com/macrovector) - www.freepik.com

> Dead Code: Hey. I'm already dead. Why you killing me again? Leave me alone

> Developer: You can't just exist here because you are dead. You gotta be non
> existent. It's time
