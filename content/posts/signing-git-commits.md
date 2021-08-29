---
title: "Signing Git Commits"
date: 2021-08-26T20:31:30+05:30
draft: true
---

Before we get started, this post is NOT about the `Signed-off-by` message in git. Now let's get started ;)

Signing Git Commits? What is "signing"? How can you "sign" git commits?

Long ago I had written about [Digital Signatures](https://karuppiah7890.github.io/blog/posts/digital-signatures/). When I sign, I mean digitally sign. So we are talking about digitally signing git commits over here

You can read more about digital signatures in my previous blog post - https://karuppiah7890.github.io/blog/posts/digital-signatures/ and more about it online too, as I'll not be going over digital signatures in-depth in this blog post

So, before we go into how to sign git commits, first, why would someone sign git commits? It's for the same reason anyone would formally sign anything - to claim that it was them who signed it and acknowledged it and what not. And here in the context of git commits, if a person wants to digitally sign git commits, it will be to show that they were the one who made the commits, assuming that no one else has the key to digitally sign the commits. So, no one can impersonate the person as anyone verifying will look for a signature to see if it really was committed by the said person mentioned in the git commit metadata

I'm not sure why anyone would impersonate someone else and do a git commit with someone else's name and email ID. One reason I can think of from the top of my head now is - if there's an attacker with malicious intent, they can impersonate as someone else who is reputable and contribute code to some project, with some probably hidden malicious code in it, and get it into the project. This *could* happen. So, to prevent someone else from impersonating you and committing code and raising Pull Requests / Merge Requests or directly pushing code to the project, it puts you at risk if the code is problematic - malicious, buggy etc. It shouldn't be possible for anyone to impersonate you, as that can be pretty scary for various reasons. I can't think of any other reason / situation now where git commit signing would otherwise help. It can surely show that "this commit is a trusted commit" which also ties back to the previous point where attackers cannot impersonate you and commit code. I think it's pretty obvious that signing can help prevent impersonation / forgery and the above scenario could be considered a popular problematic scenario which can push people to use git commit signing and prevent the problematic situation

So, how does this all work out? The git commit signing that is.

Like any digital signature, git commit is signed in such a way that only the signing authority has access to sign the commit, and no one else can (in an ideal situation), and others can verify who signed the git commit

So, how does one do it? As a developer what should one do to use git commit signing? Well, it's a bit of work, but once done, it should just work always from that point on

Like popular digital signatures, git commit signing is based on public key cryptography. So you have a public key and a private key. People call this as GPG keys, where GPG is short for GNU PG, that is GNU Privacy Guard. Now with the private GPG key one can sign commits and with the public GPG key, one can verify the commits they have received to ensure that all the commits have a valid signature

You can find this whole process well documented in most source code hosting websites, like GitHub for example and also online. I'm going to be giving an explanation of each step before doing the step

Like I said, you need keys, for signing and verification. So to create keys, specifically GPG keys here, you can use `gpg` command

You can follow a nice doc here - https://docs.github.com/en/github/authenticating-to-github/managing-commit-signature-verification/generating-a-new-gpg-key

I usually just use this command -

```bash
$ gpg --full-generate-key
```

This generates a key pair - public and private key, by asking a lot of questions about the keys. For example, what kind of keys you want, this is basically about what kind of encryption and decryption cryptography algorithm you want to use and hence create keys accordingly for that algorithm, and it's also about what you will use the keys for, signing, encryption. It's weird though, signing is also encryption, just with private key, hmm. Anyways. Depending on the algorithm selection, you may also be asked to input some parameters related to the algorithm, like, for RSA, you will be asked how many bits long the RSA keys have to be, some value between 1024-4096 bits. For ECC - Elliptic Curve Cryptography,y you will be asked to select an elliptic curve, looks like there are many. So, similar to this, you may need to provide more input. After that you will be asked how long the key is valid for, I usually just choose - "does not expire" :P 

You will then be asked your name, email address and any comments. For example a comment like "Company Laptop gpg key for company email ID" or whatever

It's important to provde the same name and email in the above step that you use in git for `user.name` and `user.email`. This is important for the git commit signing to work


