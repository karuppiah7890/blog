---
title: "Creating Dummy Files"
date: 2020-03-20T18:09:41+05:30
---

I have created dummy files two times or so now. Now, what do I mean when I say a
dummy file? In my case, I mean a file which doesn't have any actual content,
instead it has more like some random dummy data. Similar to the
[lorem ipsum](https://en.wikipedia.org/wiki/Lorem_ipsum) content that people use
for placeholder text during development when they don't have actual content
ready yet

Before telling the reasons for why I needed a dummy file, I'll first tell how I
created a dummy file. I have created it in two ways now, one is using dd,
another is using fallocate which I tried just today! Both of these I had to run
in Linux. I also found out how to create dummy files in Mac OS and Windows OS
too! See below

Works on Linux and Mac OS too:

```
$ dd if=/dev/zero of=dummy-file bs=1 count=10000
$ ls -alh dummy-file
```

That will create `bs x count` bytes, which is `1 x 10000 = 10000 bytes`. You can
check more about the `dd` command in your OS by using `man dd` or `dd --help` 

`dd` is what I used long ago, and I remember it being quite fast. But today I
noticed one more tool that was fast too, and apparently it's really really
fast compared to `dd`. It's called `fallocate`. This is how I used it

Works only on Linux:

```
$ fallocate -l 10K dummy-file
$ ls -alh dummy-file
```

For my use case, I created files of the size `1900MB`.

Below is what I tried in MacOS, the `mkfile` command but it doesn't seem to use
up disk space, based on what I see in `df` and `du`, but `ls` shows that the
file is big. May be use `dd` which will lower the disk space by consuming space.
I need to research more on this command 😅

```
$ mkfile -n 1g dummy-file
$ ls -alh dummy-file
```

Some asciinema demos in Linux and in Mac:

Linx:

{{<asciinema 311980>}}

Mac OS:

{{<asciinema 311984>}}

See below reference links for information on how to do it in Windows,
and about where I learned about how to do it in Linux and Mac OS -

[stackoverflow: Quickly Create a large file on a Windows system](https://stackoverflow.com/questions/982659/quickly-create-large-file-on-a-windows-system)

[stackoverflow: Quickly Create a large file on a Mac OS X system](https://stackoverflow.com/questions/26796729/quickly-create-a-large-file-on-a-mac-os-x-system)

[stackoverflow: Quickly Create a large file on a Linux system](https://stackoverflow.com/questions/257844/quickly-create-a-large-file-on-a-linux-system#5688625)


So, why did I even need dummy files in the first place? Well, today I needed a
dummy file which is of a large size in a rabbitmq pod's attached volume in
kubernetes, so that it can use up disk space and when a lot of disk space is
used, it will in turn trigger an alert from our alert system, which I wanted to
see and test. Cool right? Putting pressure on your disk by creating large dummy
files ;) Our alert was not showing info correctly and I wanted to fix that, so
I was simulating the situation like this. And the alert was generated using
[Prometheus](https://prometheus.io) and sent to our [Slack](https://slack.com)
alerts channel using
[Alertmanager](https://prometheus.io/docs/alerting/alertmanager/). If you are
into infrastructure tools and monitoring, these are some well known tools to
check out ;)

That's all I had to tell about creating dummy files :)

Some more reference links that I used:

https://duckduckgo.com/?t=ffab&q=dummy+file+linux&ia=web

https://unix.stackexchange.com/questions/33629/how-can-i-populate-a-file-with-random-data

https://www.cyberciti.biz/faq/howto-create-lage-files-with-dd-command/

https://duckduckgo.com/?t=ffab&q=quickly+create+large+file+in+macos&ia=web&iax=qa
