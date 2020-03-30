---
title: "uff Tool"
date: 2020-03-30T08:03:52+05:30
---

### tldr

Install `uff` and check it out! Below is an example!

```
$ go get -u -v github.com/karuppiah7890/uff
$ git clone https://github.com/fishworks/fish-food
$ cd fish-food
$ uff Food/helm.lua 2.14.2
existing food version: 2.14.1
upgrading fish food Food/helm.lua to version 2.14.2 ...
done! 🐠
```

GitHub repo for the plugin - https://github.com/karuppiah7890/uff

### Longer version

Sometime ago I created a tool called `uff`. The repo is
[here](https://github.com/karuppiah7890/uff)

In this post I'm going to tell the story behind the tool. First the
what, then the why, and a bit of how.

#### The What

`uff` is short for `update fish food`. So, what does that mean? I'm sure you
would have used package managers - for Operating Systems.
[`gofish`](https://gofi.sh) is one such package manager. I use Mac OS, and I
like it when my package manager is fast. So I started trying out the fast
`gofish`. And then I found out that the index of `gofish` is a git repo and
each package is called `food` and the git repo is called `rig` and the
packages are maintained only by a few people's contributions. So, I started
contributing to the repo, which is - https://github.com/fishworks/fish-food .
Now, each food was represented in a lua file. I don't know lua really. But I
just got the hang of how to update a package to a new version and how some
simple things work by seeing the source code in the food lua file. It was not
too hard to guess some things. 

`uff` tool is meant to help you update fish food to a newer version.

#### The Why

Initially I was manually updating the lua file with information. This
information includes -
1. New version
2. SHA sum for each OS's artifact for integrity check

It was easy to update the version. I knew there was a new version available by
checking the github repo of the application. For SHA sum, I found out the URL
for the artifact from the code - it was an interpolated string. And then
downloaded the artifacts for different Operating Systems and then found the
SHA sum. I think I did this a few times for some packages. Probably 2-3 times
or more I guess. Can't recall now.

I finally realized it was too much of a task to do so much for a small PR. I
thought it had to be easier than this. So, I thought I'll automate it for my own
good and for ease of contribution. That's how I created `uff`. That's the
reason for creating `uff` - to automate, to make my life easier.

#### The How

I'll tell a bit of the how I created the tool.

Now, I wanted to interact with the food file, to update it to a newer version.
This was a lua file I was dealing with. I hadn't done that before. I was
thinking about how to read from it. I was not sure if I can use golang (I do a
lot of golang) or if I needed to use lua. Now, `gofish` was written in golang,
so, I thought I should be able to parse the lua file in golang if `gofish` can
do it. I then checked `gofish` source code to found out how it interacts with
the lua file. There was a golang based lua package to help with that. Finally,
in my code, I leveraged `gofish`'s golang package functions to read the food
file and use the abstractions and types it had defined. Now, this was easy.
I was able to get the current version and the URLs of the artifacts for
the current version and their SHA sums. But now I needed to update the version,
get the new URLs of the artifacts and their SHA sums. I realized that `gofish`
always only reads from the food file after evaluating it, it didn't write to
the food file. 

I was confused about how to write to the lua source code file with new version value
for it's version variable and then evaluate it to get the new URLs, as the URLs
were interpolated strings with version as one of the variables and usually the
name being the other variable. This would require reading the lua file as is,
parse it without evaluating it, and then change the variable value and then
write back to the lua file, and then read it and evaluate it to get what I
needed. I searched for sometime but couldn't find a way to do it.

Finally this is what I did - I thought of a hack. Since I wanted to change only
the version variable's value to newer version, I did it with blind string
replace. Now the only thing remaining was to update the SHA checksums for the newer
version's artifacts. There are different ways to obtain the SHA checksum - some
artifacts have a checksum file hosted too where some kind of naming convention
is maintained to show that the file is a checksum file. For example if
`artifact.tgz` is the artifact, the checksum file is hosted as 
`artifact.tgz.sha256`. Example: Check the assets in this GitHub
release - https://github.com/kubernetes/minikube/releases/tag/v1.9.0 .
One can download this checksum file to find out the SHA sum. And there could be
different SHA checksums other than SHA 256 checksum, so that a possible issue, as `gofish`
packages always have only SHA 256 checksum. But yes, this was an easy way - download
one small file and that's it. But then, it's not necessary not all artifacts
will have this - it depends on who provides the artifact which is not under our
control. Also, I remember some softwares providing SHA checksums for multiple
artifacts together in one file instead of separate files. Example - check this
https://releases.hashicorp.com/vault/1.3.2/vault_1.3.2_SHA256SUMS file present
along with the other artifacts here https://releases.hashicorp.com/vault/1.3.2 .

So, it was clear that the above ways are not deterministic way to find the SHA
sum, but of course are a good way because the amount of data to download is very
very less in the above methods. Since it's not deterministic, I decided to
download the actual artifact and calculate the SHA 256 checksum in the code. 
For this, I read from the lua file and evaluated it to get the new URLs for the
new versions and then downloaded them one by one, found the SHA 256 checksum and
then for writing to the lua file - I again decided to do blind string replace -
I knew the old version SHA 256 checksum, and the new one, so it was straight
forward to do this

And that's it! The automation was over and I was able to easily update packages
with just a single command

#### Some missing pieces and thoughts for the future

There are still some things that I could do better. The string replace looks
still hacky to me, but that's okay. I guess it might be a lot of effort to fix
that. But there are some missing pieces to the existing automation, even with
`uff`, this is how I update packages now

1. Check if a new release is present in github repo
2. Pull the latest code from fish-food repo
3. Checkout to a new branch for the PR
4. Use `uff` to update the package to newer version
5. Git add, Git commit, Git push
6. Raise a PR in github

I was planning to automate this too. I just didn't do it yet. The plan for each
of the points are -
1. For checking out new release in github, github has a watch for releases
feature for users - but that only sends mails to users. Need to check if there's
any API for this - on a push basis. Or else the brute force way is - keep
polling for the latest release and check if it's a new one by comparing it with
the one in fish food

2. Use golang code to
    - pull the latest code from fish-food repo
    - checkout to new branch
    - use `uff` package to do it's update
    - git add, git commit, git push
    - interact with github api to raise PR

Another thing is - I always have old branches in my forked repo that have been
merged in the main repo. But I don't delete these branches. They usually get
reused later. Anyways, it would be better if they are deleted, or cleaned up
somehow. 

For this automation - I don't know if a background service would be needed. Felt
like it might be needed. Need to check on that. And if it's going to long
running - is it gonna run in my local whenever my laptop is on, or is it gonna
be the cloud, like, probably heroku free tier. Let's see. But if I do that,
that would be cool :D :D

Some edge cases I can think of are -
1. Raising multiple PRs for the same food. This is possible when the old PR for
the food's new version has not been merged yet and the automation raises another
PR for the same new version - this should not happen, this is an error from the
tool's automation perspective. And another possible thing is - PR for same food
but newer version. Let's say the tool raises PR for v1.1.0 to v1.2.0, and that's
not merged and then it raises another PR for v1.1.0 to v1.3.0 , ideally, the
old PR needs to be closed in this case.
