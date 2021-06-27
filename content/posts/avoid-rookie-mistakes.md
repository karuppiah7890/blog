---
title: "Avoiding Rookie Mistakes"
date: 2021-06-27T11:43:20+05:30
---

This is a short post about how one can try to avoid rookie mistakes. This is just me jotting down lessons for myself :)

> Note: Rookie mistakes are common and can happen anytime. It's good to learn from them and create processes and tools that help you succeed :)

Recently I made a rookie mistake where I created a temporary git branch in a git repository, but I couldn't delete it and this lead to problematic things that I won't get into

What I learned from this experience is this -

## Revertible actions and safety net

It's always good to ensure that any action one performs, that action can be revertible. This way, if there's an issue due to the action, the action can be reverted. This is assuming that when the action is reverted, the issue goes away

For example, if you plan to delete a virtual machine running in production - based on the fact that it has to decommissioned and hence removed. For starters, it's good to simply shut down the virtual machine, or just stop the processes in the virtual machine, for example server processes. Now, in case you acted upon the wrong virtual machine for some reason, you can still start the processes, or start the virtual machine in case it is shutdown. Or let's say you chose the right virtual machine and were decommissioning properly, but let's say someone is having production issues due to this, and are complaining that the decommissioning process was not known etc (can happen for various reasons), it's all okay as you can still revert things by turning on the virtual machine or starting the processes in it. Later, after some cool off period, the virtual machines can be decommissioned

Whenever some big operations happen - big changes, or deletions, it's good to have a safety net like the above - a way to go back to the old state.

Another popular example is - deleting databases in a database server, or deleting tables. All these are generally considered very dangerous actions. Let's say you connect to the wrong environment - the production environment, instead of staging environment and delete some table, that's a big problem. Usually what people do is - take backup of the data that they are deleting before deleting it or doing similar dangerous actions. I also remember a colleague who used to do database actions in a transaction, for example in Postgres, and then one can abort the transaction if something goes sideways, or commit it if all is good

So, it's always good to have some sort of safety net - a way to revert any issues that can happen, a way to revert your actions that can revert any issues that can happen :)

Note that if you are able to revert your actions but not able to revert or fix any issues that happened due to your actions, that's still a problem! :/ So, beware!!

In my case, my revert action is - delete temporary branch once I'm done. But the problem was, I couldn't delete the temporary branch due to permission issues - only admins had that permission and in general no one was allowed to delete git branches. This happened mainly because I assumed I can delete the branch, or didn't even think about it or check if I can delete the branch, which leads me to the next section

## Validate assumptions

It's always good to know your assumptions and validate them. For example, when I was working, I assumed that I can delete the git temporary branch, only to realize later I couldn't

I could have either asked someone about it, or tried out a very minimal thing probably, to check if temporary branches can be created and also deleted. For example by creating the branch but not committing anything in it, something like that, and then deleting it. It's still a problematic / risky approach

## Read the documentation

Another big mistake I made was - I was reading code and trying things out. I missed to read the documentation. When trying things out, I noticed an error, which lead me to try a different method which in turn lead me to do actions that weren't revertible and that became a problem

It's always good to be patient and read the documentation before jumping into new tools / environments

## Get help when things go wrong

When things went wrong, I asked for help and fortunately got help! It's good to ask for help and let people know when things go wrong :) Teams are usually there to help each other and fix problems that happen, so don't hesitate to ask for some help :)

## Conclusion

It's very possible that no matter what lessons you learn, new issues / problems / mistakes happen, and you learn new things

I hope that your team and environment allows you to experiment and make mistakes with a safety net and learn from them :)
