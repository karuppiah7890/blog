---
title: "Bytes #4: Please don't use plain text files to store secrets!"
date: 2021-04-02T05:07:35+05:30
---

Based on some recent experience, I have noticed how one can easily store secrets in plain text files and things could go very wrong.

First off, some people store secrets in plain text files in their git repositories by mistake

Only for example purposes, below are some links

https://github.com/UnravelAI/unravel-api/commit/c0599c4d798c8333b8a1c643d1e02dbec4a7cc1d

https://github.com/steppingcloudlabs/single-tenant-alumniportal/commit/f99e126fd1d8e486ac81e2e3d77aa14424ad525f

I just had to do a GitHub search

https://github.com/search?q=aws+credentials+env&type=commits

And I found the above. Fortunately the people who removed the keys from their git repository knew that the git history would contain it and hence revoked the keys and rotated it to get new keys. Those are AWS cloud credentials / keys. This can happen to any kind of secrets - Private keys of SSL certificates, access tokens to different kinds of systems like cloud, communications (email, chat like slack, discord etc).

It's very important to keep your secrets safe - which means - one should not expose it publicly on public source code repositories. Some people encrypt their secret files / content instead, if they really want to commit it to their code repository. For example [Travis CI](https://travis-ci.org/) config allows you to encrypt your secrets with a key - https://docs.travis-ci.com/user/encryption-keys/

And even if your git repository is private, don't put your secrets in it. In case your repository account gets hacked or repository gets hacked or if you make your repository public later by mistake or intentionally but forgetting that it has secrets, then your secrets are exposed!

So, that's one way to expose your secrets by mistake - by committing into git repositories

What else can go wrong? The next thing is about having secrets in your local

When storing secrets in your local, please try to not use plain text files. Again, use encrypted files, or another good alternative is to use the operating system's keychain which usually stores all secretive stuff.

If you are building a tool for your users, and if it is going to store secrets like tokens, passwords etc, then use the operating system's keychain in an automated way to store that secret and save your users from malicious programs and hackers.

NodeJs has a nice library on npm called [keytar](https://www.npmjs.com/package/keytar) to help with storing passwords in the operating system keychain and it has support for MacOS, Linux and Windows

Look for similar libraries / tools and use them :) Please do not store your secrets in plain text files! 🙏

Fun fact: Did you know that by default AWS CLI tool stores your secrets in a plain text file in your home directory in `.aws/credentials` ?

{{< tweet 1377618770076794881 >}}
