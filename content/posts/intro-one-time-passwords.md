---
title: "Introduction to One Time Passwords (OTPs)"
date: 2020-09-22T21:02:50+05:30
---

> Disclaimer: I'm no security expert. The post is more from a techy user's
perspective, with little research.

I'm usually always into guessing what goes on behind the scenes of some
technology and then checking online more about it and then understanding how
it's actually done.

So, in this post, I'm going to be talking about One Time Passwords (OTPs) and going to
try to take a crack at how it is probably implemented behind the scenes. I have
learned a bit about it before, but not implemented a complete end to end
solution - I mostly used libraries or existing services to implement the
feature, and I haven't learned all there's about to learn about OTPs. But I'm
going to take a shot at trying to explain from a user's point of view - a user
who is a techy and has been using OTPs for quite some time now.

With that said, let's dive into the topic straight. I'm sure a lot of you must
have used OTPs, given you already have access to the Internet and are reading
this post, written in English. So you are well read and have enough money to
use the Internet, so it's very possible that you have used OTPs somewhere on
some site or some mobile app.

So, where all is OTP used? What does OTP help in? What's so special about it?
Let's start by answering these questions.

From my experience, I have seen OTPs being used in multiple places -
* For bank transactions - apart from the debit card details, banks additionally
use OTP as a 2nd factor authentication
* For easy signup and login for users of an application, using their mobile
number, instead of using email ID
* For verifying your phone number, similar to how applications verify your email
ID. More like a one time thing for phone verification
* For 2nd or nth factor authentication as part of any standard authentication
flow

The usage of OTPs is just booming. You will see OTPs being used in a lot of
applications if you are using the modern day applications (websites and mobile
apps)

Let's dig deep and understand the different ways OTP helps in.

OTPs as the name says is a One Time thing. Actually, the word "One Time" can be
tricky as it's really based on the implementation. It can be configured in such
a way that, once the password is used, it cannot be used again at all. There is
also a time factor to OTP at times, depending upon the implementation - where a
person can use the OTP any number of times in the given time frame. Usually the
time frame is very small, depending upon the use case - like 1 minute,
5 minutes, 15 minutes, 30 minutes etc. I don't think I have seen anything more
than 20 or 30 minutes. This time factor helps the user to have enough time to
receive the OTP and type in the OTP. If you want to keep the system very very
secure, reduce the OTP expiration time to a very low value, but still a good
enough value to let the user experience be good - you don't want to timeout
so soon that the user has no time to input the OTP :P

So we see that OTPs are pretty secure as they are one time thing with a short
expiration usually. So, that's really helpful! It's surely something special
about OTP, as usual passwords are set once and it stays the same forever, unless
the user changes it. Only in some cases, some systems prompt / force the user to
change the password once in every few months, and have some strict policies for
not reusing old passwords. The expiration for the password is pretty high
compared to OTP.

Each kind of password has it's own use case, but I have noticed in some systems
where OTPs are used to replace the standard passwords and not augment it as
part of 2nd factor authentication.

What else can an OTP help in? OTPs can help in verifying mobile numbers, or
even emails for that matter. Usually we get those verification emails to verify
our email ID right? It has a special link, which you click and voila, your
email is verified! The link usually has some special code in it, which is
similar to an OTP. And a similar mechanism is used to verify phone numbers.

The flow goes something like this

> **Application**: Hey, I want to verify your phone number. Give me your phone
number.
>
> **User**: Okay, here you go `+1234567890`

> **Application**: Okay, I'm sending the One Time Password to your phone through SMS
>
> **User**: Okay

Application creates a OTP, say `908196`. Creates a text message with the OTP.
Application remembers that, for number `+1234567890` , the OTP created is
`908196`.

> **Application**: Hey Network Provider, send this SMS to `+1234567890`
>
> **Network Provider**: Sure thing. It will be done!

User receives the SMS, sees the OTP `908196`

> **User**: Hey, this is the OTP - 908196
>
> **Application**: Let me verify it

Application verifies based on it's memory, if the OTP is right.

> **Application thinks**:
> 
> This user mentioned their phone number is `+1234567890`.
> The OTP I created for the number `+1234567890` is `908196`, and I sent it
> through SMS. User gave the input as `908196`.
> 
> `908196` (OTP I created) is equal to `908196` (User input) ✅
> 
> Cool! The user does have access to this phone number!


> **Application**: Hey, the OTP is right. Your mobile number has been verified!
> 
> **User**: Yay! Thanks!

That's how a basic OTP flow looks like for mobile number verification and it's
similar for few other use cases too.

Now, let's look at some possible tweaks or configurations here
* What is the format for an OTP? Is it all numbers? How long can it be? - I have
seen a lot of OTPs in the form of pure numbers. Especially the ones that are
sent through SMS. And it's usually 4 digits or 6 digits. But it doesn't stop
you from implementing a version which uses alpha numeric characters - alphabets
and numbers together. I think numbers are easier for the user to see and type.
Sometimes, if the application has access to SMS and can read from the SMS, then
that can be done too. In which case, you can use any format for your OTP as the
app is going to automatically read from SMS. But if user denies access to the
app to read SMS, it will be hard for the user to read and type alpha numeric
OTPs *I think*. I would *think twice* to use special characters like `,`, `*`
etc or even special language characters like `á` etc in OTPs. That's more like
a proper password with lots of crazy characters, rather than a simple OTP.

* How is the OTP sent to the user? In which channel? - I have noticed applications 
following different ways to do this. In the above case, it's an SMS. It can also
be over email. I have noticed bank applications send OTPs for transactions to
both mobile number and email. Some people prefer to use email as it's
**faster**, in such cases, where the bank transactions have a particular
expiration period / time out for the whole transaction. I have also noticed
applications sending OTP through automated voice calls, as an alternative to SMS.
Something to note is - it is very clear that, there needs to be some sort of
connectivity to some network - mobile network - for SMS, calls, or Internet /
intranet for emails. There are some special OTPs, that can work completely
offline, with no connectivity. We will come to it! :)

Now that we have seen some tweaks in such a flow, where OTP is used for phone
number verification, what does it mean to verify the phone number in this way?
I think it's a good way to verify phone numbers. *But*. Something to note is -
this only verifies that the user who claims that they own the phone number -
what we have verified is that they have *access to the phone number*. If for
some reason, someone somehow hacked / hijacked / got access to someone else's
SMS / Calls, then they can verify themselves as someone else - more like
masquerading.

It's kind of a lot of effort or maybe even impossible for an application to know
if the physical person accessing the application is really the person who owns
the phone number - like, has their name is registered to the phone number in the
Network Provider's records. I mean, it could be easily like this - I'm using my
phone, accessing an application, but I use my dad's mobile number to login as my
dad and I get access to his SMS for the OTP and then I can help my dad by doing
some work on behalf of him in the application. I just have access to the phone
number and don't own it.

There are better ways to verify phone number too. I have noticed some
applications, for example
[UPI](https://en.wikipedia.org/wiki/Unified_Payments_Interface) applicatons in
India, which is money related, and hence needs maximum security do this in a
different way. They check your phone number by using the application to access
your SMS and then send an SMS from your phone using the SIM which has the number
that needs to be verified. The SMS is sent to an external number, which then
receives the SMS and then an application which has access to this external
number's SMS inbox, reads from it and does verification of your phone number.
And there's probably more to it, this was just an example based on experience.

Now that we have seen some ways in which OTP works and helps and is special too
because of the way it helps. Some reasons why you would use OTP - it is the
same thing as the ways in which OTP is used - if you have a matching use case
and it makes sense for your application - go ahead, use it!

I think that OTPs over SMS / Calls is pretty ubiqutous probably because of the
way our phone is too close to us, and hence our phone number. Not all of us - all
ages, all kinds of people, from different countries, cultures etc, would be
using something like email and email inboxes, and read emails. But a lot of us
would surely call people using phones, receive calls, probably send and/ read
SMS, probably use WhatsApp. Given this fact, a lot of the applications are
trying to get a hold of the phone numbers of users, to send Ads, promotions, etc
through SMS, calls, WhatsApp and any medium that links to your phone number.

For some applications, phone number is probably a key element in the
application. For example - transport apps like Uber, Ola (in India) need to
know the phone number of their user to be able to connect to them and to also
let the drivers and riders connect to each other through phone calls. There's
also Internet calls these days, in such apps, to avoid exposing phone numbers
of the users to other people. In any case, some of these apps, use phone
numbers as the main way to uniquely identify a user and also have some extra
details - the usual email ID, and anything else that makes sense for the
application. And applications accordingly restrict any other user to reuse the
phone number or even the email address, and also make sure that there's some
sort of security - for example, by saying that only one user with a particular
phone number can be logged into only one device at a time. Just an example.
There's probably more work behind the scenes to take care of such use cases,
but verifying the phone number is part of the big process.

Now, we spoke about different things about OTPs. There are some special cool
OTPs. I know two of them only - TOTP - Time based One Time Password, HOTP - 
HMAC based One Time Password.

I can only talk about TOTP as I have used it a lot and read about it.
Probably I can do some follow up writings about HOTP later.

Remember I mentioned about some OTPs not need network connectivity for it's
usage? TOTP is one such OTP. Let me give an example of how it works based on
how I have used it.

For TOTP to work, there are some pre-requisites. The user should be known to the
application before hand, before using the TOTP. Or let's just say that there
has to be some sort of initial communication needed between the user and the
application, for the TOTP initial setup, and then the TOTPs can be used later.
And any time the setup is again needed, a similar communication has to happen
again. So, it is kind of like a pre-requisite setup before starting to use TOTPs.

Usually I have seen TOTP used in 2nd Factor Authentication (2FA) - which is a
feature few applications provide, where the user provides two different things to
identify themselves. In general also called Multi Factor Authentication (MFA).
For this feature, the user first signs up for the application, logs in, and then
sets up their 2FA, in the process, setting up a way for providing TOTPs, as TOTP
is what is used for the 2FA. So, in this case, the application should know the
user, through signup, then login - to setup the TOTP.

Now, TOTP works based on Time, as the name suggests. The user and the
application share a unique secret, and this secret is different for different
users. For example, for me and application `A`, our shared secret could be `ABCDEF`.
For my friend and application `A`, they might be sharing a different secret,
say `PQRSTUV`. This is just an example. The application remembers each user's
secret key, that the application and user share. The user **also** remembers the
same key on their side. Let's look at an example / demo of how TOTP works.

**The First meet between the user and the application**

> **karuppiah7890**: Hey, let's setup a way to identify me using TOTPs, for my
> 2FA
>
> **Application**: Sure! Let's have a shared key!
>
> **karuppiah7890**: Sure!

Application generates a shared key `LOLOLRTEFQ`

> **Application**: Here you go, remember this key `LOLOLRTEFQ` in your TOTP
> application
>
> **karuppiah7890**: Okay! I'll use Authy app to store this key and remember it!
>
> **karuppiah7890**: Hey! I stored it!
>
> **Application**: Cool! You can use TOTP as an additional thing to identify
> yourself next time, apart from your standard password!

The Application remembers that for user `karuppiah7890`, the shared secret key
is `LOLOLRTEFQ`.

The verification process looks something like this. This can happen anytime,
after the initial setup.

**The verification stage of TOTP**

> **karuppiah7890**: hey, it's me, karuppiah7890
>
> **Application**: Yeah, I know, because you provided the standard password to
> identify yourself. But I need something extra, your TOTP.
>
> **karuppiah7890**: Sure thing. Let me get the TOTP from my Authy app!
> **karuppiah7890**: Hey my TOTP is `606347`. It's going to expire soon! Oops.
> Log me in fast! Or I need to type again.
>
> **Application**: Just a sec.

Application does it's standard math calculation for TOTP - which takes two
inputs - Current Time and the shared secret, which is `LOLOLRTEFQ` in this
case as that's what has been stored for the user `karuppiah7890`. After the
calculation, it gets the value - `606347`. The user provided input `606347` and
the calculated value `606347`, both match ✅

> **Applcation**: Yup, I see you are in fact karuppiah7890, unless you hacked
> karuppiah7890 and got access to their shared secret key
>
> **karuppiah7890**: Nah, it's actually me! Chill! :)

As you can see, there's some calculation that's being done to get the value of
TOTP. This calculation is universal - and that's why the user's app - Authy
was able to do it and provide a value to the user, as it's hard for the user,
a human to do such big calculations - I'm not going to go into the details of
the calculation, as even I don't know much :P But I do know this (or have
noticed this) -

Input to calculation: Current Time, Secret Key
Output: 6 digit numbers

At least that's what I have noticed. If you are new to this, I would recommend
to think about what kind of function would you use to do this calculation. Also,
I said `Time`, but `Time` has timezone attribute to it, what if the user and
the application are in different timezone? :P Maybe they all use the universal
UTC timezone for their calculations? Go checkout it out ;)

Also, the TOTPs are valid for 1 minute or so usually. So, when I say the
calculation is done with time, the value is still the same for a period of time,
I think.

I think I'll do a follow up write up on more about TOTP and HOTP :)

That's all I had to talk about OTPs. I hope you had a good basic introduction
to OTPs in general! Do let me know your thoughts in comments or any questions
you have!

Looks like there are some good links on the Internet for these topics
https://en.wikipedia.org/wiki/One-time_password
https://en.wikipedia.org/wiki/Time-based_One-time_Password_algorithm
https://en.wikipedia.org/wiki/HMAC-based_One-time_Password_algorithm
