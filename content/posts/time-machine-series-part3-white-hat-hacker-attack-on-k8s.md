---
title: "Time Machine Series Part 3: White Hat Hacker Attack on K8s"
date: 2021-09-01T22:12:30+05:30
draft: true
---

One of my team matest allowed "system:anonymous" user to access everything using RBAC - role binding and role

It was a public cluster - API server IP was public

One of the white hat hackers from the client hacked it and reported it

Another colleague immediately deleted the cluster due to fear. It was an unnecessary cluster so he just did it

Later we were told that it was a knee-jerk move and stopping the attack by deleting the cluster was not the right way

The security team from client side told that we shouldn't erase evidence - logs, running pods etc where we can find data about what the hacker did. If it was a black hat hacker, a malicious hacker, then it could have been worse. Evidence helps to understand the attack's impact - what was attacked, what happened, what the hacker did, what assets were compromised, what was the complete implact with some number / value - like money lost etc


