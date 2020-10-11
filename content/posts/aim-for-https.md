---
title: "Aim for HTTPS"
date: 2020-10-11T09:13:50+05:30
---

## TLDR;

- Be aware of the benefits of HTTPS.
- **Always** try to use HTTPS when using a website
- **Always** try to use HTTPS when building / developing a website, even if
  your site does not get any sensitive or confidential input from the user at all, or store or serve any user data.

## Longer version

If you have not heard of HTTPS before but have heard of HTTP, I would recommend
reading about it. A simple one liner is - HTTPS is the secured version of plain
HTTP. You could check out the [HTTPS Wikipedia page](https://en.wikipedia.org/wiki/HTTPS)

If you haven't heard of HTTP, you can first read about HTTP and then about
HTTPS :) HTTP is a prominent protocol of the web :D

Now, if you know both HTTP and HTTPS, read ahead! :)

Like any other software developer, I browse the Internet quite a lot. In my
experience as a user browsing the Internet has been mostly great. I usually use
the Firefox Developer Edition browser. Let me tell you two of the bad
experiences I have had.

I'll start with a common bad experience. I used to use a website that had only
HTTP. I don't want to mention the site name, but this was a very simple
website - for solving some interesting coding problems and then submit answers
and discuss solutions in forums, and there was ranking and other stuff. So yes,
it clearly had a user database with login feature so that each user sees their
data on the website. The craziest thing about plain HTTP is that - as you
already know - the protocol is not secure, so all the communication between the
client and the server is all plain text. So, in case anyone hacks into this
communication by acting as a middle person - famously known as
[Man In The Middle Attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack),
then they can actually hack into all the data that I get from the server. This
is my data, present on the server. They can also hack and find out my password
which I use when I login. Now the website has moved to HTTPS. I actually didn't
worry much about using the website as I didn't have too much knowledge about
the issues with HTTP. Thankfully I used very dumb passwords in the website and
didn't reuse that password for important websites. Back in the day I just used
a few set of passwords without password manager - some for important and secure
stuff, others for unimportant ones like entertainment related but has no money
or any serious data.

Now a days, browsers help with such HTTP issues by indicating that the website
is not "secure" when it uses plain HTTP. Some examples from my Firefox
Developer Edition is below

When I try to type some credentials / secret information, say username or
password in a HTTP website
![input-in-insecure-website](/blog/img/aim-for-https/input-in-insecure-website.png "input-in-insecure-website")

When I'm on a HTTP website, the URL bar shows this. I think many must be aware
about this if they are aware a bit about HTTPS
![insecure-website](/blog/img/aim-for-https/insecure-website.png "insecure-website")

I think this is pretty common knowledge now - Do not use HTTP, and always use
HTTPS in case you have a login or any user input for that matter, especially
when the user input is sensitive data like email ID or phone number which is
private information of the user. Also, use HTTPS when you have user data that
you serve to the user. It's good to be aware of the problems if you use HTTP in
such cases. Read more about it online if you aren't already aware! :) HTTPS is
the way to go for secure communication if you really care about your users.
Also, your users will trust and use your website only if you care about
them and their data :)

Now, the less common thing. One might say "Okay, so, if a website does NOT get
any input from the user and does NOT have login or does NOT store or serve user
data, and is just a simple plain static website, then it can on HTTP right?"

At first glance, you and I may think it can be on HTTP, yes. That's a very
simple and straight forward thing. But let me tell you - even such sites can
face the problem of a bad user experience and even security issues. Let me tell
you how!

I was browsing the Internet and landed on some HTTP page, I can't remember
which one. But it was a simple website, I wasn't logging into it or anything,
just reading some article. Suddenly, a popup came out from the bottom right of
the website. It showed my Internet Service Provider's logo and showed all my
details and reminded me to pay the bill! I was like "what??". It took me quite
sometime to realize that it was a proper legitimate thing, because it had all
the details and I realized how my ISP possibly did it. And this has happened to
me quite a few times now 🤦 The first time was creepy though.

Let me tell you what they did - since I'm connecting to the Internet using my
Internet Service Provider - they can actually monitor my Internet traffic. What
they did is - they found out that the website I was using was HTTP - meaning it
is not secure and since they were in the middle, kind of like the Man in the
Middle attack, when the website's server sent a response, they injected their
code into the website with my details, to show a popup to me, to remind me to
pay the bill as my bill due date was nearing. Crazy right?

Now, this is a very simple thing, probably an annoying thing, but it doesn't
harm anyone. If your website uses HTTP, and I use it, just an annoying popup
will come on your website telling me to pay my Internet bill. All because my
Internet Service Provider is crazy and also because your website uses HTTP and
not HTTPS. Let me tell a worst case scenario now. Your website is famous and you
still use HTTP - you don't have any user data, or login or anything. It's
probably just something like a blog let's say. Your users trust you and your
content. One fine day, I'm using your famous website, over a public WiFi (like
a coffee shop WiFi), where many know the WiFi password and are on the same
network. Some hack-for-fun kid could easily do a Man in the Middle attack to me
when I'm using your website. The kid could just insert some popup saying "Enter
your email ID to subscribe to our newsletter" or anything like that. If I trust
your website and content, I might actually type it out! And boom! Someone could
easily trick me and get my email ID. This is just an example. It need not be a
kid, it could be a serious hacker, injecting the wrong links on your website and
luring your users to a hacked website, and similar things. Imagination is the
boundary - many bad things can still be done in case your website is HTTP.

So, even if your website is a very very simple website, with nothing but simple
information, and no input, login or anything for the user to give their data, I
would recommend you to use HTTPS based on the above reasons :) Also, even if the
user tries to use HTTP - force them to use HTTPS by doing a redirection to the
HTTPS version of the website. This is a common technique.

Using HTTPS is not very hard given the rise of
[Lets Encrypt](https://letsencrypt.org/), a Certificate Authority (CA) that
issues Free SSL/TLS Certificates to websites. These SSL/TLS Certificates are
needed to serve your website using HTTPS. Only well known and trusted
Certificate Authorities can issue trusted SSL/TLS Certificates for websites on
the public Internet. Usually people used to pay and get SSL/TLS Certificates
from Certificate Authorities, to use HTTPS. Some companies have their own well
known Certificate Authority and use it to issue SSL/TLS Certificates to their
websites, for example [Google Trust Services](https://pki.goog/). And now we
have a free Certificate Authority to issue free SSL/TLS Certificates for using
HTTPS :)

As a user, beware when you use HTTP websites! Most of the websites use HTTPS
today, so there are probably only a few that use HTTP, or may use both HTTP and
HTTPS and may not force the user to use HTTPS.

If you have any questions or comments, do add them below! :)
