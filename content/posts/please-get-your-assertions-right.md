---
title: "Please get your assertions right!"
date: 2021-03-27T19:42:35+05:30
---

## TLDR

Assertions in programming generally have "expected" and "actual" value to check
if the "actual" value is equal to the "expected" value. When the programmer
provides the two values in the wrong order to the assertion library then when
errors occur, they are confusing to read. Getting the order of "expected" and
"actual" value can be helpful to get a sensible error message which one can
read and understand the issue when errors occur in assertion.

But when there are no errors, that is when the two values are equal, the order
doesn't matter and you don't face any issues even with the wrong order, as both
values are equal, there are no errors, and no confusing error messages to show.

Please get your assertions right by using the right order of expected and
actual values, so that when things go wrong / when assertion errors occurs,
people reading the error are not confused. Let's avoid confusions!

I know linters which help with finding such nitty gritty issues! :) For example,
in Java, I have noticed Sonarlint to find such issues - [Sonarlint Java rule for
correct order of assertion arguments](https://rules.sonarsource.com/java/RSPEC-3415)

## Longer version

In this post I want to talk about a very small thing that I have interacted with
for a long time now and sometimes ended up in a confused state because of it.

I want to talk about test assertions in this post. If you do software
development and haven't done testing or haven't heard of assertions, I would
recommend you to check it out, it's an interesting thing! :) I'll give a brief
introduction to it based on whatever little I know :)

## Assertions: A short introduction

The concept of assertions can come up anywhere and not just in the testing
domain, but I believe it's popular in the testing domain.

So, what is an assertion?

An assertion in the programming world is a statement (line of code) where you
assert (the English dictionary meaning) and say that a statement Must be true
or Must hold. If that's not the case, the program throws an error

[Assertions Wikipedia page](<https://en.wikipedia.org/wiki/Assertion_(software_development)>)

Let's see an example in Java using JUnit testing framework's `assertEquals`
method

```java
int value = 1;
assertEquals(1, value); // this passes
assertEquals(2, value); // this fails
```

If you are not familiar with Java, I hope it's still understandable and
readable. Also, many languages have similar assertion libraries with such
functions. These could be within testing frameworks or even standalone outside
of a testing framework. You could check them out

Back to the statements. The statements with `assertEquals` are called assert
statements or assertions.

What does`assertEquals` do? It asserts or checks if the two values are equal or
else throws an error.

This kind of checking or assertion is popularly used while writing automation
tests for a software. A usual assertion in programming for testing a
software would look like this in the form of English

```markdown
- Hey software take this input and do this work
- Give me the output
- Let me check if the output you gave me is the expected output
```

Now, if the software is not working right, it would give the wrong output or
what's called an unexpected output. In general, as you may already know,
anything unexpected behavior in the software world is generally called an issue
/ bug. The process of testing software is used to catch such bugs / issues as
soon as they appear in the software.

## Getting your assertions right

Now, let's get to what do I mean by getting your assertions right.

Before doing this, let me show an example output of a assertion when an error
occurs

```java
int value = 1;
assertEquals(2, value);
```

```bash
org.opentest4j.AssertionFailedError:
Expected :2
Actual   :1
```

Look at the output and notice how it says that the expected value is 2 but the
actual is 1. What does this mean?

Remember what I told about assertions and testing? Recall the part -
"Let me check if the output you gave me is the expected output"

So, what does it mean when we say - `assertEquals(2, value);` ?

It's not just about checking if the two values are equal. The two values have a
name too. If you check the definition of the different `assertEquals`
overloaded methods you will find the names of these values by checking the name
of the method parameters in the method definition.

For the above method, the [definition](https://junit.org/junit5/docs/5.0.1/api/org/junit/jupiter/api/Assertions.html#assertEquals-int-int-) looks like this

```java
// Asserts that expected and actual are equal.
static void assertEquals(int expected, int actual)
```

Look at how it says `expected` and `actual`. So, according to `assertEquals`,
you tell it two values, one is the value that you want to check, which is the
actual and the other is the expected value - the value you expect the other
value (actual) to have.

Notice the order of arguments here. In this assertion method the expected comes
first and then the actual comes. Each assertion library has it's own way of
defining how to provide the expected and actual, but it's something to take
note of. This is what I want to talk about in this post.

The tricky part is - when both expected and actual are same, you get no errors
and everyone's happy.

When they both are not the same, you get an error like the above, which is just
an example.

So, what can go wrong here? Well, the teeny tiny bit that can go wrong or weird
over here is, typing code something like

```java
int value = 1;
assertEquals(value, 2);
```

What the above means is - "I'm expecting the actual value `2` to be equal to the
expected value `value`"

Why is the above not so good? The thing is, assertions are usually written in
such a way that - the value of expected is a constant or somewhat a constant -
derived from something that's constant and value of actual is the value you get
from the processing / work done by the software. While testing, you test when
the software changes. And software generally changes more than your test code.
So, it's very possible that when bugs occur, the value of actual changes in
an unexpected manner - as the software has a bug. But the expected will remain
more of a constant. Let's see how the errors in your test would look like
during different scenarios

Let's say you have a calculator module.

```java
int value = calculator.add(1, 1);
assertEquals(value, 2);
```

When the calculator has a bug and it's output value for addition of 1 and 1 is
1 instead of 2 and then you see

```bash
Expected 1
Actual 2
```

That's a weird error. Someone reading the error will be like "But 1 + 1 is 2, 
why does it say expected 1? And actual 2 is correct right?" only to realize 
that the assertion statement is wrong - the order of arguments for expected and 
actual is wrong

This kind of confusion happens only when the assertions fail, that is usually 
during errors. But when the assertion passes, in this case when the two values
are equal, the order of arguments doesn't matter. Only for error messages the 
order of arguments is pretty important.

And I have been doing software development for 3 years now. It's pretty less 
but I have come to realize pretty early that it's good to have clear error 
messages so that it's easy to find the problem and fix the issue than be caught 
up in unnecessary confusions and having unnecessary cognitive overhead and 
waste of time because of the confusion

So, please get your assertions right! :) 🙏
