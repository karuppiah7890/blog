---
title: "Code snippets on Internet"
date: 2021-08-21T12:39:30+05:30
---

I think this is very intuitive and straight forward topic. Pretty cliche too. But I think it's important to talk about it nevertheless

There are so many technical articles on the Internet. Lot of technical content. Many of them also include code. I have seen different websites display code in many different ways. In my blog, the code display is very very basic actually 🙈 But I have high expectations from other people now 🙈 Let's go over them!

# Higlighting the code in the code snippet

This is a pretty neat and important feature I think. But yeah, a feature that's useful only for people using their eyes a lot, not sure about differently abled folks.

Highlighting helps people read code snippets with ease, and give an experience similar to how they read in their IDEs, probably with a different theme though. Some websites allow to change theme too for code snippets. This helps people to process the code and understand the syntax and the code, as it's all properly highlighted

# Avoid using images for code snippets

I have seen quite some articles use images for code snippets. This is usually crazy. I mean, it's hard for people to copy paste the code snippet if it's in the form of an image. Only an Optical Character Recoginition (OCR) kind of tool can convert images with text to text. It's also a very inaccessible way to share code snippets for people who have difficulty reading and use screen readers.

# Updated and working code

Now, this is a hard thing. I must say I really don't keep all my code snippets updated or ensure it's working. But I want to call out that many websites out there are doing serious business around their technical content, and even earning money, unlike me, I just blog for fun and to share :P So it's best for them to always ensure that their code is at least working and maybe even up to date with the latest versions etc, or at least mention which versions of the software and tooling was used at the time of the writing. This helps people understand that the code they are looking at may not be what they are looking for because of a version difference, it may either be too new or too old and outdated. Also, it's annoying when the code doesn't work 😅 Atleast people need more information to understand why it wouldn't work, which could due to many reasons, like, due to some version difference issue or something else too. Provide as much context as you can so that your users know what they are working with and the context

# Copy code button

A lot of websites now a days have the cool feature to copy a code snippet at the click of a button. It's pretty cool ! I mean, yeah, it might sound like a lazy thing, but sometimes copying code, especially large snippets can be hard, you would have to select a lot of the code and then copy it. It's easier with the single click of a button!

One thing to note though - it's good to have a feedback for the user that code has been copied once you copy it into their clip board. So, if you are implementing this, something like a `Copied!` text on top of the botton or on the button to show that code has been copied would help. Also, if the copy failed, due to some reason, it's better to indicate that too to the user, for better feedback on the action that they performed, which is "copy code"

Another thing to note is - it's important to be careful about what you copy. It's good to copy just what's needed, though the code representation on the site may differ. For example, if you are showing the code you are running on a shell, basically a command, then when you show it, you would show something like -

```bash
$ ls -al
```

I just used `ls` command as an example. Now, for this code snippet, when copied, it's best to not copy the `$`. The `$` is meant to represent the fact that the code is being run in a command line and that it's a command line thing. `$` is more of a visual representation but the actual content or actual code is just `ls -al`. So, beware of what you copy into the clipboard. Also, I have noticed some websites use fancy characters to show different things, which are not the exact right characters to be used in the code, as it would not be recognized as a normal valid character. So beware of that too. If a user copies the code and pastes, it should work, without giving them headaches :P

Another thing to note is - in case of command line code, it's best to copy it properly without any new lines at the end. For example, for -

```
$ mkdir -p some-directory
```

If the user copies and pastes into their command line, it should just paste it and not run it. For it to NOT run, the clipboard should contain just `mkdir -p some-directory` and not `mkdir -p some-directory\n`, note the `\n` new line character. If the new line is there, then that would create the effect of an `Enter` key on the keyboard for the command line and it would run the command. Note that this behavior differs from terminal to terminal and even shell to shell. For example I have heard `zsh` avoids running the command, but in my `bash` in built-in Mac OS `Terminal` app, I noticed it run the command. So I say it's best to not copy the new line, as the user may want to read the command before running it, or just modify the command before running it and what not

# Ability to change the code snipped

This is a feature that I have seen some websites have, where you can also edit the code, all in the article, from your web browser. This can help in case you want to change the code and copy it and paste it in another browser tab where an online compiler is running. But yeah, this is a very nice to have feature, not sure what else people would need the edit feature for, as they could copy and edit somewhere else too. One example of this is CodePen snippet, and there are many more. But, this edit feature could be useful when tied with the next thing that I'm going to talk - running code

# Running the code snippet

This is a really cool feature, where you get to run the code all from the browser. Mix this with the editing code snippet feature, and you basically have an online IDE within your article. CodePen snippet example applies here too! And of course there are many more!

# Examples

I want to use this section to mention some examples of sites and tools that can help with some of the above features

Ytt - https://carvel.dev/ytt/ , it has an example playground - https://carvel.dev/ytt/#playground where one can change the code, run the code, see output, add files etc. Pretty cool stuff! It also has a full screen mode!

JSBin - https://jsbin.com

CodePen - https://codepen.io/

CodeSandbox - https://codesandbox.io/

JSFiddle - https://jsfiddle.net/

There are many more out there!

There are more of them that allow you to play in their platform and also shared and embed those playgrounds in your articles!

GitHub Gist allows you to embed simple code snippets - https://gist.github.com/ , no running etc, but it has highlighting and other cool features - versioning, forks etc

# Conclusion

I think it's cool to show code snippets and also maybe link to them or link to the complete repo in technical articles involving code. Above are some things I usually notice when I'm reading technical articles online and I think many of them are pretty cool and provide a nice user experience
