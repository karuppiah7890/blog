---
title: "VSCode Golang Programming"
date: 2020-03-17T14:10:35+05:30
---

I usually use IntelliJ when I'm working on company projects. These days I have
been trying to use VS Code. Recently I have realized that VS Code's Golang
extension has a lot of new features to offer. I think a lot of credit goes to
gopls ("go please") - https://github.com/golang/tools/tree/master/gopls . I have
been using VS Code with repos that have go modules (without vendoring). Most of
the features present in IntelliJ are now available in VS Code too for Golang.
Below are the things that I usually use while programming in Golang, so,
disclaimer - it is **NOT** an exhaustive list of features present in VS Cocde. My
intention of this blog post is to show that a lot of things are now possible in
VS Code too :D I have **not** mentioned how to do the same in IntelliJ. I will
however mention what features are missing in VS Code but are present in IntelliJ
and some downsides of IntelliJ that I have seen. Let's get started with the
features present in VS Code and how to use them. I have recorded a video to show
the demo and also written about it :)

Note: The shortcuts are based on MacOS keyboard. Lots of the commands don't
have shortcuts by default - you can assign custom keyboard shortcuts to them
according to your own wish

Video:

I thought this would be a small video, for like 10 mins or so. But as usual I
got over enthusiastic and over did things and made a bit of a big video :P

{{<youtube Zk6IQC3ojf8>}}

Text Form of the Content (with almost the same information):

**Find all usages for a variable / function / method**

When you are navigating through a codebase, reading code, or debugging, you will
usually want to know where all a variable is being used - being read from,
written to. Or even check where all a function is being used - where the
function calls are done. For this -

You can use `References: Find All References` from the Command Palette

You can also use `Peek References` from the Command Palette

And you can use these commands when the cursor is on the variable / function
definition and also when the cursor is on the variable / function usage too! :)

Peek shows the result in the same place where you are without moving around
much - to another file or same file another place or anything and not on the
left on the references bar

**Find all implementations for an interface**

You can use `References: Find All Implementations` from the Command Palette

You can also use `Peek Implementations` from the Command Palette

Why is this a cool and important feature? Let me tell you.
Golang interfaces are a great concept! And as you already know, Golang
interfaces are based on the concept of loose coupling, unlike some languages
like Java - where types explicitly say if they implement an interface. In Java,
you can easily find all implementation for an interface by even using `grep` and
search for the name of the interface and you will find some search results with
`implements` keyword and the interface name and then you will know which types
implement the interace. 

But in Golang, there's no explicit definition to say a type implements an
interface, but if a type has all the methods of an interface with the same method
defintion, then that type implements the interface. This is all good, but how
does one find all the implementations of an interface that they see in a Golang
codebase? No worries, that's where VS Code has got you covered ;) Using the
above commands it's very easy to find all the implementations of an interface :)

**Rename a variable / function / method**

A lot of times you would want to rename some variable names or function or
method names - for better naming or to avoid clashes in naming and for many
more reasons. Renaming should not be hard. VS Code has got you covered here
too ;)

You can use `Rename Symbol: F2` from the Command Palette

After typing new name, use `Shift + Enter` to preview, and again `Shift + Enter`
to rename. You can choose (tick) where to change the name and untick others and
then choose to rename. This also renames comments present for exported
variables. 

Note: Renaming a method in an interface does **NOT** rename all the
implementations.

**Extract a variable**

Sometimes you will find big expressions. For better readability. With VS Code,
now you can extract an expression to a variable. 

You can use `Go: Extract to Variable` from the Command Palette

I have noticed this to not work in some cases - I don't know why 🙈Some weird
import errors have come when doing it sometimes. But I think VS Code will get
there and fix all those problems. Or may be we can all Go and contribute to
the Golang extension and to `gopls`, because, Why Not? ;) Why May be? I think
I must contribute, for all the benefits it gives :P

**Go to Definition**

This is like a usual and basic feature that everyone wants. I'm looking at a
variable, I want to know where it has been defined, the same for a function
or a type or a package or anything for that matter. VS Code can help you with
that!

You can use `Go to Definition: F12` from the Command Palette.

You can also use `Peek Definition` from the Command Palette

**Go to Type Definition**

This is something new I just noticed. Now, let's say you are looking at a
variable, and want to know what's the type of it and it's definition, VS Code
has a feature to help you with that too!

You can use `Go to Type Definition` from the Command Palette

You can also use `Peek Type Definition` from the Command Palette

**Go backward - navigation**

When you are navigating a codebase, jumping to defintions (instead of just
peeking) and reading code and going into rabbit holes, you could get lost easily
and be like - where did you even start from? VS Code has got you covered here
too! You can go back with ease :)

You can use `Go back: Ctrl + -` from the Command Palette

**Go forward - navigation**

If going backward is so easy, going forward and again into the rabbit hole that
you went through a few moments ago, should be easy too. VS Code helps you
Go(lang) forward too :P ;)

You can use `Go forward: Ctrl + Shift + -` from the Command Palette

Note: VS Code really does make you go (Go) forward - to help you speed up your
development, in this case with Golang, and I think even in other languages like
JavaScript, TypeScript and more! VS Code's popularity has been increasing quite
a lot and I'm not surprised :) It's pretty good.

**Toggle to the test file**

Do you write tests? If you are not, do consider writing tests :) If you are the
hard core testing person, you will want to checkout test files. When you are
seeing a code and want to see if it has a test file, you can use this feature.

You can use `Go: Toggle Test File` from the Command Palette

If there's no test file, it will give an option to create the test file.
I don't know how this exactly works, but just because the feature says there
are no test files - don't assume the code you are seeing is not tested. To be
sure if tests are present or not, I would recommend checking all the references
of the function or method you are looking at and see if there are any test files
in the references and check those references in the test files. In many cases
this feature has however helped me :)

**Run all tests in a test file**

You are seeing a test file, you want to run all the tests that are present in
the file, how do you do it? It's easy with VS Code! :)

You can use `Go: Test File` from the Command Palette

**Run a test function**

You are seeing one test in a test file and want to run that alone. Again, very
easy with VS Code :)

VS Code adds two buttons just above the test functions - `run test` and
`debug test`. You can click `run test` and it will run the test.

Note: You can **NOT** run a single `t.Run()` with VS Code. VS Code does not
show run and debug options for `t.Run()` function calls

**Show code coverage**

You have done TDD or written tests, but how do you check how much tests you have
written? Which code did you test and which one you missed? You can check this
by checking the code coverage and VS Code can help you with that too!

You can use `Go: Toggle Coverage in Current Package` from the Command Palette

This shows the code in green and red to show which code has been tested (green)
and which code has not been tested (red) at the current package level

By the way, how much tests is too much tests? :P Comment below :P

---

Now that we have seen all the features that I use that VS Code provides, some of the
features that I couldn't find or make it work are given below. These are
features that IntelliJ has. I belive VS Code will get there and might even
beat IntelliJ ? 🤔 Let's wait and see

**Extract to function**

VS Code does have a command called `Go: Extract to function`, but I was not
able to see it work. Need to keep digging. If I make it work, I'll add it to
the above list ;)

**Move a function to another package**

IntelliJ has this really cool feature to move a function from one package
to another and all the references to the function also get fixed along with
imports and what not. It's really cool ;)

**Run one t.Run() test**

In IntelliJ, we can run exactly one `t.Run()` function instead of the whole
lot of `t.Run()`s in a single test

**Change method or function signature**

IntelliJ also has this feature to change the signature of a function, where
you can change the parameter type, name (this one must be easy, it's like
renaming variable within the function), add extra parameters - add default
value for the parameter for all the usages of the function, and then add
return types too. Awesome right? :)

---

I'm hoping that soon VS Code will have these features too. I'll soon go and
raise issues for these features if they are not already present in the github
issues for VS Code and/ `gopls` 

That's all I had to talk about Golang programming in VS Code
