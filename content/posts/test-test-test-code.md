---
title: "Test Test Test Code"
date: 2020-03-16T19:22:28+05:30
---

I have contributed to a few open source repositories. One good thing I learned is - tests are very important
and that we need to make sure that the software is robust. And when software breaks, it means that some
test for a scenario is missing - add it, and the test will fail, as there's a bug and then it can be fixed.
Now, that's a fix being done after the bug has been discovered. Now, how do you make sure that your code
has less and less bugs? More like precaution rather than cure, cure being - fixing the bug after discovering
it. Sometimes the bugs can turn out to be pretty costly 😅 For precaution - you can do this by making sure
that all your scenarios are covered as much as possible in your tests. Now how do you do that? Make sure
test coverage is good. Meaning, whatever code you write, make sure that, that code is tested. Now, I have
heard people say that 100% coverage isn't possible, agreed, just try your best. Also, I think I
have heard that 100% is also not a good number 🤔 I don't really know too much. I need to read on that.
Based on my experience, which is two years of professional experience, I can say this - usually I haven't
seen people test their code a lot, like I haven't seen 100% or some really big number. But tests will be
there, yes. And I have seen bugs come up in these softwares. So, it comes back to what I was talking about
test coverage - but how do you ensure that whatever code you have written is tested as much as possible?
Especially unit tests. And I have more experience only with those. I have not done integration tests or
functional tests much, so can't say much about them. Back to unit tests. Now, a thing that I have usually
seen is - people write the code first, then write tests for their code. I have done it too, a lot. What's
the problem with that you ask? Well, there are two problems that I know of -
1. It's very easy to miss out testing some parts of the code
2. Sometimes testing is hard. So, at some point, we might even skip the tests (!!!). If it's small and
simple code, that's fine. But if it's more than just a few lines of code, you might have to consider
testing it to see some value in the long term

Now, how can you get rid of these problems? One thing I do is, I finish off the tough stuff first - write 
the test first. A failing test. And then pass the test by writing the code for it. If you have already 
heard of it, yes, this is what people call Test Driven Development (TDD). Also, when you write code - you
should write as minimal code as possible to pass the test. It's not easy 😅 but it's possible usually. When
you do this, any code you usually write is tested - this is because, you first write the test, then
write just enough code to pass the test. If you write more, then that code is something that's not tested
and hence, even if you remove it, your tests will pass. So, make sure you write just enough code to pass
the failing test. Check more about TDD online :) And now your code is robust as it's tested. You can also
do manual testing once you are done and it will surely be working or you can write some more tests to cover
any issues / scenarios that you find, which you probably missed before. The possibility of bugs to occur
now is less. If it does occur, you will write more tests. Initially it will look like a lot of effort to
write tests - do check the effort vs value. For now, I'm always trying to write tests, whatever the code.
Sometimes it's a lot of effort 😅🙈 Anyways, I believe that when the code evolves in the future, the tests 
will surely help people who are contributing in the future. They will check out the tests, probably modify
it for the new feature and test the new feature and write tests for bug scenarios too. It's all for the 
good, unlike the case where there are no tests and newbies won't know that they need to do it and when they 
try to learn by example - they will find that there are already no tests, so they don't learn anything new
and might feel tests are not needed, which isn't great, and then the maintainer of the repository has to
come and request for tests after the Pull Request is raised, in the case of an Open Source repository. If
you are a maintainer of a repo or an open source contributor, I would request and recommend you to advocate
for tests, TDD, and robust and bug free software. Also advocate for simplicity :) 

If you still aren't convinced about tests and TDD, check [this open source story](https://karuppiah7890.github.io/blog/posts/oss-stories-part-1-write-tests-to-fix-that-bug/) I wrote, talking about an
issue that could have been avoided through tests - 

https://karuppiah7890.github.io/blog/posts/oss-stories-part-1-write-tests-to-fix-that-bug/
