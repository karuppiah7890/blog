---
title: "Jabba: A Java Version Manager"
date: 2020-12-05T15:02:57+05:30
---

### TLDR;

Try [Jabba](https://github.com/shyiko/jabba), an Open Source Tool for managing
multiple Java versions in your system. It's written in Golang. It has been
inspired by [nvm](https://github.com/nvm-sh/nvm) which manages Node versions

### Video Version

Before going into the post, if you prefer videos over blogs, you can checkout
this YouTube video, which has the same information as this blog post.

{{<youtube mGYj96lcEGw>}}

This blog post will be mostly the text version of the video :)

### Longer Version

Do you work on Java projects? Do you have multiple Java versions across
different projects? Yes? How do you manage it? No? You are probably lucky :P

In this blog post I want to talk about a dev tool called
[Jabba](https://github.com/shyiko/jabba), which is a Java version manager.
There are probably other alternatives, but Jabba was the first hit I got when I
searched for Java version manager

https://github.com/search?utf8=%E2%9C%93&q=java%20version%20manager

[Jenv](https://github.com/jenv/jenv) was one alternative that I heard of. I
guess their description didn't help with getting them on the top of the GitHub
search result but they have lots of stars on GitHub. I'll probably try it in the
future. Anyways, for this blog post, I'm going to focus on Jabba :)

### Why?

I think the reason for the existence of such language version managers maybe a
bit puzzling. I want to give a gist to those who might think "Why do I even need
this?"

Now, I'm someone whose always trying to keep things - so I usually would love it
if I just had to install one version of a compiler or interpreter - preferably
the latest version and always be able to update whenever new versions come. I
do that for my Golang installation. I always update to the latest actually. But
then, why would I go and use something like a Java version manager or even Node
version manager for my node projects sometimes.

The biggest thing to understand is - let's say if I want to always use only one
version and preferably the latest version, then, all the source code of my
projects should be compatible with it and work with it. So, what I need to do
is -

- Update to latest version of the language (compiler / interpreter etc)
- Update the source code if needed to keep it compatible with the language
  version

Now this is where the issue comes in. Sometimes it may not be easy to simply use
a new version of the language with your source code. Your source code may need
some changes to keep it compatible with the latest language version. In such
cases, instead of spending time to make the source code compatible, the
alternative is to go ahead and use multiple language versions for different
projects. So, very old projects may be using older version because at the start
of the project, it may have been the language version and maybe no one tried to
spend time to upgrade the language version and probably the source code too. But
when starting new projects, they would try to go with newer language versions.
So, at the end, you end up having multiple language versions.

One good thing to note is that, many languages try to not have breaking changes
in their language spec. Java rarely does breaking changes from what I know and
I have heard that it's one reason many people love Java. The same goes for
Golang I think, except in one case where I noticed some breaking changes in
Golang's generated files (go.sum).

Now that I have told you one example of why would someone even need multiple
versions, let's move on to Jabba!

### Jabba

Jabba is pretty straight forward to use. It's written in Golang and you can get
a single binary and put it anywhere and it will run. I use Mac OS and I
installed it with this command

```bash
$ curl -sL https://github.com/shyiko/jabba/raw/master/install.sh | bash && . ~/.jabba/jabba.sh
```

For other ways to install or other OSes, go to https://github.com/shyiko/jabba
and check installation methods

After installation, use `jabba` in the command line and it will help you with
it's help instructions

```bash
$ jabba
Java Version Manager (https://github.com/shyiko/jabba).

Usage:
  jabba [flags]
  jabba [command]

Available Commands:
  install     Download and install JDK
  uninstall   Uninstall JDK
  link        Resolve or update a link
  unlink      Delete a link
  use         Modify PATH & JAVA_HOME to use specific JDK
  current     Display currently 'use'ed version
  ls          List installed versions
  ls-remote   List remote versions available for install
  deactivate  Undo effects of `jabba` on current shell
  alias       Resolve or update an alias
  unalias     Delete an alias
  which       Display path to installed JDK

Flags:
  -h, --help      help for jabba
      --version   version of jabba

Use "jabba [command] --help" for more information about a command.
```

Before using Jabba, I would recommend getting rid of any other JDK
installations, so that you have lesser confusions and also lesser other Java
binaries in your `$PATH` environment variable

To check what Java versions / variants you can install, just check using Jabba
like this

```bash
$ jabba ls-remote
1.15.0
1.15.0-1
1.6.65
adopt@1.15.0-1
adopt@1.15.0-0
adopt@1.14.0-2
...
...
adopt@1.8.0-181
adopt@1.8.0-172
adopt-openj9@1.15.0-1
adopt-openj9@1.15.0-0
...
...
adopt-openj9@1.8.0-242
adopt-openj9@1.8.0-232
amazon-corretto@1.11.0-9.12.1
amazon-corretto@1.11.0-9.11.1
...
...
amazon-corretto@1.8.232-09.1
amazon-corretto@1.8.222-10.1
graalvm@20.3.0
graalvm@20.2.0
...
...
graalvm@1.0.0-6
graalvm@1.0.0-5
graalvm-ce-java11@20.3.0
graalvm-ce-java11@20.2.0
...
...
graalvm-ce-java8@1.0.0-6
graalvm-ce-java8@1.0.0-5
liberica@1.15.0
liberica@1.15.0-1
...
...
liberica@1.8.232-10
liberica@1.8.222
openjdk@1.15.0
openjdk@1.15.0-1
openjdk@1.14.0
...
...
openjdk@1.9.0
openjdk@1.9.0-4
zulu@1.16.0-0
zulu@1.15.0
...
...
zulu@1.7.60
zulu@1.7.55
```

As you can see, there are so many versions and variants available. You can
choose one and then install it. For example, like this

```bash
$ jabba install openjdk@1.11.0-2
$ jabba install openjdk@1.15.0-1
```

Jabba has been inspired by [nvm](https://github.com/nvm-sh/nvm), so if you have
used `nvm`, you would find a similar experience with `jabba` :)

To check what are the installed Java versions, just run

```bash
$ jabba ls
openjdk@1.15.0-1
openjdk@1.11.0-2
```

To use a Java version, you can run

```bash
$ jabba use openjdk@1.11.0-2
$ java --version
openjdk 11.0.2 2019-01-15
OpenJDK Runtime Environment 18.9 (build 11.0.2+9)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.2+9, mixed mode)
$ which java
/Users/karuppiahn/.jabba/jdk/openjdk@1.11.0-2/Contents/Home/bin/java

$ jabba use openjdk@1.15.0-1
$ java --version
openjdk 15.0.1 2020-10-20
OpenJDK Runtime Environment (build 15.0.1+9-18)
OpenJDK 64-Bit Server VM (build 15.0.1+9-18, mixed mode, sharing)
$ which java
/Users/karuppiahn/.jabba/jdk/openjdk@1.15.0-1/Contents/Home/bin/java
```

You can also check where your JDK is installed

```bash
$ jabba ls
openjdk@1.15.0-1
openjdk@1.11.0-2
$ jabba which openjdk@1.15.0-1
/Users/karuppiahn/.jabba/jdk/openjdk@1.15.0-1
```

Also, when opening a new shell / terminal, to be able to set one default Java
version, you can use the jabba alias like this

```bash
$ jabba alias default openjdk@1.15.0-1
default -> /Users/karuppiahn/.jabba/jdk/openjdk@1.15.0-1
$ jabba alias default
openjdk@1.15.0-1
```

So, every time you open a new shell, it will have the default Java version. You
can also use `.jabbarc` to store your Java version in your different
repositories, and then use `jabba` like this

```bash
$ echo "openjdk@1.15.0-1" > .jabbarc
$ jabba use
$ java --version
openjdk 15.0.1 2020-10-20
OpenJDK Runtime Environment (build 15.0.1+9-18)
OpenJDK 64-Bit Server VM (build 15.0.1+9-18, mixed mode, sharing)
```

### Conclusion

Those were just some of the features of `jabba`. There are a few more. You can
check other features of `jabba` by checking it's help :)

```bash
$ jabba --help
```

Jabba seems to be a pretty simple tool to use and mostly intuitive too. Let me
know what you think!

If you have any questions or comments, do comment below :)
