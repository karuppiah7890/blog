---
title: "Role Based Access Control"
date: 2020-11-15T20:02:34+05:30
---

Role Based Access Control, also known as RBAC is method to provide / restrict
access to users in a software system. It's something that comes up in the topic
of Authorization.

If you haven't already heard about Authentication and Authorization, it's worth
to check out both of them. The one liner is, Authentication is all about
identifying the user, it answers the question "hey user, who are you? identify
yourselves, and prove me who you tell you are." Authorization is all about
controlling the level of access the user has, with the idea that not all users
can have the same level of access, so it answers the question "hey user, what's
your level of access? do you have access to top secrets? can you do everything
in this system? or you are just a simple user with very little access to do
very basic things?".

We will be focussing on Authorization in this post, and particularly focus on
role based access control.

If you are looking to learn more about all this, I would recommend searching for
the above terms and also the concept of Identity and Access Management. Where
Identity is about Authentication (Authn) and Access is about Authorization
(Authz)

So, what is role based access control really? Let me start off with the idea
about why it's needed and what kind of problem it solves.

Let's say there's a big organization. They are using a software, which is used
by all the users in the organization. It can be any piece of software. Now,
let's say that the software has tons of features, and it also has features like
adding a new user to the system, for cases when a new employee joins the
organization, and then a feature like removing a user from the system for cases
when an employee leaves the organization and many such features. Now, what do
you think about the access to all the features of this system? Do you think that
all users can have the same level of access to this system? So, can anyone add
a new user to the system? Can anyone be able to remove a user from the system?
Is that okay? How does it happen in your organization?

I'm sure you must have used some sort of such software if you have ever been in
any kind of organization or even an institution like school and colleges.
Usually the software system has some sort of administrator who take care of
managing the addition and removal of users from the system and helping with any
issues. Right? It's kind of like, they are in God mode, haha. They are like Gods
in the software system, with access to everything. It's not like anyone in the
system can do anything. Not all users are the same. Each have different access.
Role based access control is exactly about this.

Role based access control approach is about - providing access / controlling
access based on the user's role. Let's break it down a bit more. What does
providing / controlling access mean? It means to provide or control access to
the features of the system. Not all users can do all actions - they don't have
access to perform all the actions that a system provides in terms of features.
And what does a user's role mean? Usually the system defines a set of roles for
the system and the real world also works based on roles when it comes to
organizations. For example, my role in my company is that of a software
developer. So, if the software system has a role defined as software developer,
then I would be given that role and the system would let me do only the stuff
that are relevant and can be done by software developers and not anything else,
for example, any action that can be performed by a CEO cannot be done by me.
Software systems define the roles based on this real world scenario.

Let's try to understand more about what does it mean to define a role and how
does it all really work.

Let's say I have a system for blog posts for my team, where many people
collaborate together. Let's define some of the actions for the system based on
it's features. The system allows one to write blog posts, publish blog posts,
save drafts of blog posts, review someone else's blog posts and provide
comments on blog posts, review someone else's blog post and approve it for
publishing.

My team has bloggers / writers, proof readers, editors. These are the roles of
people in my team. This is just an example. Now, let's say I'm a blogger,
ideally, my team would expect me to only write and let the proof reader do the
proof reading to look for typos and grammatical mistakes and such. And let the
editor read once to approve it for publishing, and only then I can publish it.
But my editor can publish blogs directly after letting the proof reader do the
proof reading.

In this system, one could define that any user with the role of editor can do
almost anything - write, review, publish posts. Proof readers can only read and
review and comment on posts for doing edits so that writers can get the feedback
and edit it accordingly. Writers can write posts, save draft and request editor
to approve the post for publishing after the approval.

If you notice, we have some sort of relationship here. There are users, there
are roles, and then there are actions. By the way, users can be real people,
such as in this case, or even programs (automated systems).

According to role based access control, the concept goes like this - role is
defined by it's permission(s). That is, a role has permission(s), which
indicates permission(s) to do some action(s). Users belong to a role, and hence
have the access / permission(s) to perform certain action(s).

How does this happen? Where is all this relationship data stored and used by the
system? How is it even stored? Usually, the based on the software and it's
features, authorization and role based access control must be a feature to even
be able to use it. And then, usually the first user of the system - the admin,
that is, the administrator, has all the access in the system. The system usually
has it's own set of roles and definitions for it's roles. Or, the system has a
feature to create custom roles. Or maybe both is possible. So, there would be
some sort of feature to find out what are the different roles in the system,
and their definition - their name, and the list of permission(s) under that
role, and maybe even create custom roles. Once admins have ensured that there
are different roles, based on their organization / use case, with properly
defined permissions under each role, they then move on to - mapping users to
role(s). A user may belong to one or more roles. The user gets access to all the
permission(s) under each of the role(s) they belong to. Usually the user is
already part of the system / defined in the system. If not, they have to login
and be defined in the system, so that this mapping can be done. There are also
some other ways to deal with this depending on the kind of system you are
working with - for example, I have noticed systems which can provide some
default set of role(s) to every new user who logs in, and this is done
automatically. It really depends on your use case, what works for you and your
software system.

So, in retrospect, a nice diagram would look like this -

![role-based-access-control-1](/blog/img/role-based-access-control/role-based-access-control-1.png "role-based-access-control-1")

So, users are assigned appropriate roles and hence they get the appropriate
access, like in the case of my blogging team.

This may not be a great example, given it sounds kind of like
gate keeping, but at it's core, role based access control is all about saying
that - probably not all users are the same, each belong to a role and their
access is controlled based on their role. It's kind of based on an
hierarchy concept and might mean - more power to some than others 😅

This kind of role based access control is present in a lot of modern day systems
including services that you use in your day to day life. I'm sure you must have
noticed them, maybe not have heard about role based access control though.
Some examples are GitHub, GitLab, Google Docs (and the whole Google Drive
ecosystem), cloud platforms like Google Cloud Platform, Amazon Web Services,
Digital Ocean, Microsoft Azure etc. Tons of many other examples are there. If
a system caters to a big organization or multiple teams with different roles,
it's highly possible that this concept is being used in the system with an
admin providing access, like "This user has the role of an editor. So they can
edit things. This other user has the role of a viewer, so they can only view
things and comment, and that's it."

The first time I got introduced to Role Based Access Control, the term, was
through Kubernetes which has this concept but I realized it's a widely used
concept. I also realized and thought about what else one would do to provide
access. Some of the following thoughts popped up

Why can't I give access to users directly? But then I would have to provide
granular permissions to each and every user and at some point, it might become
repetitive - mostly because some set of users need the same level of access,
mostly because of their job, their role in their job. Also, updating the set of
access is also not going to be easy when managed at a granular level for each
and every user. Something to note is, this all really depends on the kind of
system you use which has the feature to control access, for providing
authorization feature.

Role based access control can make things easier by working on access and
permissions at role level, where the admin defines the role with it's name and
permissions. And then it's just a matter of providing roles to users, also
called as mapping users to roles, so that users get the roles and hence the
permissions under those roles to perform actions in the system. And to update
permissions, it's as simple as just changing the definition of a role and
everyone with that role will have updated access.

To make things more easier, some systems also define the concept of groups -
where there is a group of users, and admins can provide roles to groups. So,
next time, a new user joins the organization, they don't have to be assigned
roles, instead, if they are just added to the user group, they get the
appropriate role and hence the appropriate permissions.

To make things more easier with respect to roles - some systems have the concept
of derived roles - or roles that are based on other roles. For example, one
could define a role as a combination of two other existing roles, instead of
adding each and every granular permission under the two existing roles, which
can become cumbersome. Sometimes they are also called compound roles.

These are just a few extra features that I have noticed.

That's how role based access control works out in a system. I just grazed over
some of the basic things I knew. If you have any questions, do comment below :)
