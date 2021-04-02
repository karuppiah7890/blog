---
title: "Bytes #3: Please don't have unnecessary high access!"
date: 2021-04-01T19:57:00+05:30
---

A lot of times it good to have a lot of access in the systems we interact with. This could be any system - external systems or internal systems. For example, git / source code repository access is a pretty common thing. Let's say your team has a private repository on [GitHub](https://github.com). You can have any kind of access to this repository - read access, write access, or even admin access, for example to change settings and what not.

Let me tell you what is a possible problem when you have unnecessary access. Let's say you have admin access to your GitHub repository even though you don't need it - you hardly change settings or anything. You just push to the git repository and need only write access.

Now one fine day, your GitHub account gets compromised by an attacker and your team's private git repository is also accessible for the attacker, and they are also able to change the settings of the git repository and do a lot of damage.

Now, how can one avoid such a possibility? Surely you need to keep your account very secure. For example, use strong password, use Multi Factor Authentication (MFA). Keep all your secrets in a safe place like a trusted password manager or any good alternatives that you know, and not in any place that can be easily exposed leading to your secrets being leaked.

To add to this, if you don't need admin access, you can get rid of it. This way, even if the attacker gets access to your account in any way, they can only get some access that you had, access that was necessary for you. But if you have unnecessary higher access, it's going to be a big lottery for the attacker!

This idea can be applied to all systems, including some very important systems like your Source code hosting platform, cloud platform. Accounts in all systems need to have just the necessary access.

If you are someone who requires high access (admin, root etc) on a day to day basis, you will have to secure your account and secrets really well. If you are someone who requires high access only at times, then get the high access temporarily and then get rid of it immediately :)

It's always better to say "I don't need that high access" than to say "My account / secrets got hacked and we lost money"

Remember the spiderman movie dialogue "With great power comes great responsibility". Same applies to great or higher access ;)
