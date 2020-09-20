---
title: "Introduction to Keycloak, Open Source Identity and Access Management System"
date: 2020-09-19T21:17:46+05:30
---

In my current project, there was a decision made to use
[Keycloak](https://www.keycloak.org/) for Identity and Access Management (IAM).
People said that "You don't need to write code to do authentication and
authorization and reinvent the wheel, it's already done by a lot of people.
Keycloak is just really good, it just works, use it".

Now, I am pretty new to almost every technology in the project. So I'm still
learning a lot of things. I recently worked on the login feature for our React
application, along with Keycloak, and it was a really good experience! So I
wanted to share about Keycloak, about what I know. Maybe I might write more on
different things I learned about Keycloak as a series, soon. There are also many
other articles on the Internet, to help you with the Keycloak. It's just that,
when I write, I might also talk about some of the things I noticed during my
experimentation, and some possible pitfalls, and also talk about a proper
possible production setup - like the whole ecosystem / infrastructure setup,
that you might need for a good user experience :)

Now, back to Keycloak. So, what is Keycloak? If you go to
https://www.keycloak.org/ , you will notice it say "Open Source Identity and
Access Management For Modern Applications and Services". It's important to note
that it's an Open Source Software (OSS). I'm someone who values Open Source a
lot. Maybe everyone might not. Some people might argue about the support,
maintenance, and the customizability of Open Source Software / Products. In
case of Keycloak - I think it's been around for a long time. According to the
[Keycloak Wikipedia](https://en.wikipedia.org/wiki/Keycloak) page, it's been
around for 6 years now! One could say time doesn't matter, and that instead
frequency of commits, quality of software matters - well, it's sponsored by
RedHat and it's apparently a pretty mature software if you look at
[Keycloak GitHub repo](https://github.com/keycloak/keycloak/) and the stars in
it, and [Keycloak JIRA](https://issues.redhat.com/projects/KEYCLOAK/) for issue
management.

If you search GitHub, which is one of the most famous Open Source Project
hosting sites, and look for "identity and access management" or
"identity management", you get these

https://github.com/search?o=desc&q=identity+and+access+management&s=stars&type=Repositories

https://github.com/search?o=desc&q=identity+management&s=stars&type=Repositories

I have sorted by "Most Stars" - maybe not a fair sort as some might say, but I
just did it. There are very few softwares that I see in this search. Probably
not the best terms to search and not the best place to search. There are so many
other source code hosting sites - GitLab, BitBucket and what not. 

Anyways, with the above search results, you will notice Keycloak to standout.
I even found another software to standout, which I have checked out a bit
before - Kratos - https://github.com/ory/kratos and the
[ORY GitHub Org](https://github.com/ory) and their
[website](https://www.ory.sh/). 

I think there might be many more softwares out there that might help you with 
authentication and authorization. For example, I know there are libraries that
help you with it - for example - [Casbin](https://casbin.org/) helps with
authorization. I haven't personally used it, but I have checked out a bit about
it. [Passportjs](http://www.passportjs.org/) that I have personally used a long
time ago when I was learning and writing NodeJs applications. I implemented login
with Google and Twitter with it. It was pretty cool! I learned a lot about
Google and Twitter apps and OAuth. It was cool! :)

Given I just spoke about so many things, one might ask me - "Why Keycloak?".
Well, the best thing is, you can get a lot of stuff done with Keycloak with just
configuration and no code. At least most of the standard things - for example,
login with Google, Twitter, GitHub, or just plain login. I didn't know about
this and I was writing code to do it three years ago, to do all that. Good thing
is, I learned a thing or two while trying to do it on my own. Anyways. Now, you
could say that - there might be some libraries to get rid of that code too,
and just have some config in your code, it's still you importing some
library and putting all that config in your code, and hey, if you haven't done 
authentication or authorization before, even if you do login with Google or
other sites, you need to store your user information and manage users, similar
to plain login. So, you need to provide database to store this information. And
also hope that your library is taking care of a lot of things - in terms of
security and is also providing the features you need. I think it's a lot for a
library to do. It's not impossible though. But anyways, I think that's where
Keycloak shines.

So, again. What is Keycloak? Keycloak is actually a server. It has an admin UI
for you to configure it. Keycloak also has a Database for storing it's data.
Now, what features does it provide? What can it help you with? It's an Identity
and Access Management software - it can help you with authentication and
authorization - what that means is that - you can secure your applications using
Keycloak - most probably without writing much code at all. So, no code to
manage users, no code to take care of all the security hacks, and other gotchas.

So, how does Keycloak help you to secure your apps? It does this in different
ways based on your application. Keycloak is not just a server. It's an ecosystem.
Keycloak provides libraries for different kinds of applications, so that your
applications can integrate / communicate with Keycloak for authentication and 
authorization. Let me give you an example, for my React application, the user
flow for login, along with Keycloak, looks something like this -

One scenario
* User goes to the React app using a URL
* User is **not** already logged in, and the React app finds this out by
communicating with Keycloak and then redirects the user to the Keycloak Login UI
* User types in their credentials
* Keycloak verifies the user's credentials
* Once the credentials are verified and correct, Keycloak redirects the user to
the React app, to the exact URL that the user was trying to go to
* If the credentials are not valid, the user stays on the Keycloak Login UI and
gets errors regarding the login.

Another scenario
* User goes to the React app
* User is already logged in, they see the app content

I think it's clear now that Keycloak also has a Login UI. A similar UI is also
used by the Keycloak admin to login to Keycloak admin UI.

Now, do you see what's cool about the above flow? You manage only your React
app, and the code you write in it - it's probably less than 10 lines of code.
Keycloak takes care of the Login UI, redirections and everything, either using
the Keycloak server or the Keycloak libraries. You might have one question now -
How does a user signup or register? Keycloak has UI for that too. It also has
features for Remember Me, Forgot Password, Session timing configuration and many
other things. I'm still learning, but this is still very impressive for me!

And you know what's the best part? You might have many use cases - like,
integrating with existing systems - Google, Twitter, GitHub, Facebook, or even
Single Sign On (SSO) systems, LDAP (Lightweight Directory Access Protocol)
systems. All this is supported by Keycloak. It's just a matter of configuration.
And plain login is supported too! Keycloak takes care of managing the users and
all the data in it's database.

Keycloak is pretty Enterprisey if you notice. They support a lot of big use
cases. Now, let's say you want to have authentication and authorization for
multiple groups of users - say multiple organizations who are your clients, with
each organization having it's own users (employees/customers/anyone).
Guess what? Keycloak can support that too! It's called Realms in the Keycloak
terminology. Each Realm has it's own set of users.

Now, one might encounter situations where the organization already has some
homegrown system for authentication and/authorization, or is using some existing
system. You could use that system directly, without ever touching Keycloak. But
let's say you do want to use Keycloak and it's ecosystem with the great features
that it provides, then you still have options. Keycloak provides so much
customization - you can just write a few lines of code using the Keycloak
libraries and create a Java Jar file, and drop it inside a path in the Keycloak
server and that's it! I'm actually loving Java (I haven't done so much Java
professionally) for the fact that a plugin system with it is just too easy with
Jar files. I have seen other plugin systems too, with Remote Procedure Call (RPC)
and other ways to communicate with core. Each has it's own pros and cons I guess,
but I did love the ease of customization in Keycloak as I did it recently to
authenticate users using an external HTTP service which is another authentication
system. Also, this kind of customization to authenticate with an external
service could also be a temporary thing, in case you want to sunset the other
old system(s). Keycloak provides documentation and strategies for those kind of
use cases too!

There's actually more customization in Keycloak - remember I spoke about the
Keycloak Login UI? You can customize the look and feel of the Login UI too!
Keycloak has this concept of Themes and you just create a custom theme with a
few lines of code and drop it inside the server and boom!

I think I have given a good gist of what Keycloak can do for you. And this was
a very very basic overview - more like how people say - 30,000 feet overview.
There are more things that can be told about Keycloak :) It's just that I'm
still learning.

By the way, I have also used or heard about other closed source paid
authentication and authorization systems, for example
[auth0](https://auth0.com/), [Firebase Auth](https://firebase.google.com/docs/auth/).
If you are into Open Source, you can use Keycloak, and I think that Keycloak is
at the level of some of these systems. I don't know much about Keycloak's
scalability though, but I have seen people talk about it and that the community
users have solved scalability problems in different ways in Keycloak.
[Keycloak X](https://www.keycloak.org/2019/10/keycloak-x.html) is something to
lookout for. So, even at scale, I think Keycloak can help. I would still
recommend you to do your research based on your use case and do your
scalability / performance testing using Keycloak :)

Few reference links:
https://medium.com/keycloak
https://medium.com/keycloak/keycloak-essentials-86254b2f1872
https://www.keycloak.org/2019/10/keycloak-x.html
