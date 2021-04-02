---
title: "How CI/CD systems work with source code hosting services"
date: 2021-04-02T06:48:35+05:30
---

I have always been curious about internals of some systems, if not all. One of them is the CI/CD system and how it works with source code hosting services. Apparently it's not too complicated. So, let's dive and see what are the different ways CI/CD systems work with source code hosting services

So, what's actually the thing that I was curious about? I was to curious to understand how a CI/CD pipeline runs for exactly a given commit especially when there are pushes to the source code immediately in a matter of a few moments / seconds, but pipeline for both runs only after a few moments but at the same time but with correct commit in each.

There are two ways that I know of in which the whole system works. One is, there is a push notification by the source code hosting service to the CI/CD system about the pushes to the source code. The other is, the CI/CD system keeps polling the source code hosting service every few intervals and keeps track of things.

That's all there is to it! :D Let's see an example

Example 1 for push (push notification)

- Karuppiah pushes code to GitHub
- GitHub notifies Travis CI pipeline that a commit was pushed with commit information preferably. Say commit sha abcd.
- Travis CI understands this and triggers a CI/CD pipeline for the commit
- The code is pulled and it's ensured that the current checked out commit is the pushed commit - commit sha abcd. This way, we are not always simply building latest commit. This is especially important when multiple commits are pushed rapidly but CI/CD system takes time to respond to the pushes. But once it does respond, it doesn't use the latest commit, instead it uses the commit for which the build was triggered
- After the CI/CD pipeline is over, the status of the commit is sent back to GitHub as GitHub supports showing build status as part of each git commit

Example 2 for polling

- Karuppiah pushes code to GitHub
- Jenkins polls the GitHub code every 10 minutes. After a few minutes it finds that there is some new code and pulls it
- Jenkins then triggers a CI/CD pipeline for the new commits
- The code is pulled and it's ensured that the current checked out commit is the latest pushed commit
- After the CI/CD pipeline is over, the status of the commit is sent back to GitHub as GitHub supports showing build status as part of each git commit


Note that both these are just examples. Some things can vary depending on the tools and services you have setup and the constraints you have.

One might ask as to - why would someone want polling instead of a good old simple push notification. The tricky part is that, it's not always that simple. For push notifications to work, there are some things to take care of. For example, you need to be able to configure push notifications (web hooks) in your source code hosting service. This means that it should have this feature and it also means that your CI/CD system should also support receiving this push notification as a feature. The CI/CD system should also be accessible from the source code hosting service - this may not always be possible, for example, the source code hosting service maybe on the public Internet but the CI/CD system maybe present in a private network behind a Virtual Private Network (VPN). This is just an example 

So, those are just some things to keep in mind. For this reason, it's possible that sometimes polling is used by the CI/CD system when push notification is not possible various reasons

## Conclusion

So, that's how CI/CD systems broadly work with source code hosting services.
