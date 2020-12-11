---
title: "Dead Code is Dead! :P"
date: 2020-12-11T23:00:35+05:30
draft: true
---

## TLDR

Dead code is just unnecessary code that's lying around. You don't need it,
nobody needs it. Just remove it and make your lives easier :) If you want to
take it up to the next level, use automated tools to detect dead code to make
your lives more easier ;)

## Longer version

Recently I have been doing some small cleanup and tech tasks in some of our
project code. This happens whenever I see something that can be improved and
have the time to fix it or at least add a TODO comment in the code, so that no
one forgets it.

There are many different kinds of cleanup and tech tasks. For me personally, the
best ones are the ones where I get to delete code :D :D

I love deleting code :P I love removing code to have less code - less things to
maintain, less things to read and understand, less complexity in our lives.
Of course, less code should still be readable 😅 It would be counter productive
to delete 10 lines of code and use 1 line which no one in my team can
understand and probably even I won't understand after a few days or months.

So, clearly I don't like having code that's unnecessary - it just leads to
problems.

Usually this kind of code is called unused code, or dead code or just
unnecessary code which is not needed any more due to some reason.

There are many kinds of examples.

Some examples are

<a href='https://www.freepik.com/vectors/background'>Background vector created by macrovector - www.freepik.com</a>

---

Notes:

Dead code. Use good tools to help you find unused code with ease. Usually quality check tools can help with this - linters and similar tools.

Unused variables, unused function parameters, unused unused methods and functions, unused classes and structs!

Also, there can be some tricky code too. Hopefully tools can help with that too! For example, you will have some source code and if you look closely, it will only be used in tests and nowhere else. Is it really needed? If not, remove it!

There can also be tricky code like

var blah = 1
blah = blah + 1

Ideally, the quality check tools must be able to identify that blah has been used only to update itself but not used anywhere else to just read the data alone and use it. So if that's the only two lines of code where blah is used, blah variable looks pretty much useless. 

Meme: blah variable : ouch. that hurt. Me: sorry...

Some warnings about deleting code - understand the code and ensure that no one is using it. For example, if the code is actually a library that's imported by others, you can never easily find out who is using it and who is not using it. Especially public external libraries, like Open Source libraries. Also remember that removal of code is usually a breaking change - it breaks backwards compatibility of the project. So ensure no one uses the code before removing it. That's one of the main definitions or attributes of dead code 😅 "it's not used by anyone"

---

Notes:

Dead code. In very rare cases, I've not removed some unused code. For example, we have end to end tests and some helper functions. These helper functions are like utils that others use. I have noticed that one or two of them are not used anymore. I could be brutal and remove them too. But I noticed that they were good utils and left it alone. It's kind of like against YAGNI (You aren't gonna need it), but I let things like this slide.

I guess now you must be like "what's this guy doing? He tells me to remove dead code. Tells me all the bad qualities it has and then chooses not to remove some dead code from his project due to some reason?"

While I'm writing, I'm actually tempted to rethink my decision and go ahead and be brutal and remove that utility function. Maybe someone else will think to write it again if they need it :p it will be some effort though, to learn some selenium and write some lines of code :p

---

Notes:

Dead code blog.

Soul Reaper trying to remove dead code. Dead code. Code is dead. Get it?

Code be like "hey. I'm already dead. Why you killing me again?"

Developers be like "you can't just exist here cuz you are dead. You are non existent. Sorry. Bye!"

Developer looking to delete code be like a hunter, looking to hunt something. Or a breaker trying to break something.

"Got you!" The developer says

"It's time" the soul Reaper says and elegantly takes the dead code away from the system. Show comics

Have some funny memes ;)

https://en.m.wikipedia.org/wiki/List_of_Soul_Reapers_in_Bleach#:~:text=Soul%20Reapers%20are%20a%20fictional,realm%20called%20the%20Soul%20Society.&text=He%20assumes%20her%20duties%20to,could%20not%20rest%2C%20called%20hollows.

---

Notes:

Dead Code is code that is unused or not executed.

Some dead code is easy to find while others might be a bit obscure.

For example, recently in our project, we decided that we don't need one of the endpoints that we have in our microservice. This endpoint was mostly used only for development purposes but we realised that we can manage things without it. Even with that decision, we took sometime to remove it, which head to some maintenance cost of maintaining tests written for the endpoint. 

---

Notes:

Dead code removal. Unused code. Clearly unused vs complicated unused. Commented out code. Fear of removing code like dead code.

IDE and lint warnings. Compiler warnings.

Simplification of code

