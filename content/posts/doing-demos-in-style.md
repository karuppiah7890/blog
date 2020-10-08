---
title: "Doing Demos in Style!"
date: 2020-10-08T20:02:58+05:30
---

## TLDR;

Record your demos and use recording to show the demo. Or at least use the recording as a backup, in case the actual demo goes wrong.

## Longer version

Have you ever had to do a demo to an audience? Say in a workshop to teach
something, or in a talk / learning session, or to show your colleagues
something cool, or just to your clients or customers in a professional setup.

Demos are what resonate with "actions speak louder than words". Most times,
your audience would love for you to show something than just use words to
describe it. Also, apparently a lot of people are good at absorbing visual
things as they think in pictures and images. A good video that explains about
this and some more kinds of people -
[Reading minds through body language | Lynne Franklin | TEDxNaperville](https://youtu.be/W3P3rT0j2gQ).

I'm not going to talk more about why demos are important or how they are cool.
In this post, I'm going to talk about other aspects of demos - more
relating to the execution of a demo.

What do you think is the best way to do demos? I think everyone might agree
that the best way to show demos is in real in-person action. But if you think
about it, it really depends on the context.

I'm going to be talking about demos in the field of software development, but
some of these concepts may apply to any field. Let's say I want to show a
demo of a software to our company's clients and show it's cool. The most basic
thing I can think of is - have the software up and running, and show our
clients the demo and also maybe give them access to it too after my demo and
let them experiment with it. This is all cool. But what happens if my
software suddenly doesn't work while I'm trying to show the demo? 😳 That
might be a little embarrassing depending on how much I boasted about the
software or the demo :P :P And seriously, if there's one thing that I have
learned in my experience as a software developer, it is - "Always double check
and verify stuff. Don't trust software systems" :P It's very possible for
software to go wrong or anything to go wrong in a sofware setup - it might be a
very small thing, but with all that in-person demo pressure, it's possible that
one might not be able to think straight to understand the problem and fix it,
or even have time to debug the issue.

What can one do in such situations? Especially given the fact that demos can be
key in some cases. For example, it could be deal breakers for your clients with
who you plan to do business. You wouldn't want to mess up the demo in such a
case, right?

To tackle such situations, I learned a trick for demos from one of my
ex-colleagues [Aswin](https://github.com/aswinkarthik). He did this one demo
for our client - by taking a video of it. It was a demo of a command line tool.
He did the demo before hand and recorded the screen while doing the demo. He
kept this video handy for our meeting with the client. The main reason was to
be able to show the demo quickly as we had only little time. This is very
easily possible with a video - which can be fast forwarded. He didn't record
any audio for the demo video, and decided to go impromptu and talk about the
demo while the video was playing.

Simple thing right? Nothing fancy. Good old recording being very handy.

Some pros of recorded demos are

- One can fast forward very easily. You could also remember or note down the
  timings of the key parts and show that alone in case of time constraints
- The recording can always be sent to the audience for later reference, and
  also to people who couldn't be present to see the demo. Maybe your clients who
  attended the business meeting can send the demo video to their bosses who
  didn't attend the business meeting ;)
- Can show actual demo too, instead of recording, and use the recording as a
  backup in case something goes wrong with the actual demo or if the actual demo
  is taking more time and there are time constraints to consider
- In general, recordings are time constraint friendly

Some cons of recorded demos are

- You cannot interact with the demo and answer any questions your audience may
  have about the demo. It's good to keep your actual running demo in such cases,
  for live interaction with running demo
- Depending on the context of the demo, live demos might be considered more
  real and believable and concrete, as we are in an era where it's easy to
  spoof things on video / recording. So the recording might be considered as
  just a mock up or a dupe, and not a real thing, especially if there is very
  less trust between the audience and the person doing the demo

This idea of recording demos can probably be applied to almost any demo and not
just software demos. For software demos, I do have some recommendations on how
to go about approaching this ideology and also about recording things. This is
purely based on my experience in the software field and also based my own
opinions.

First, consider the goals of the demo and the context. This is key to take
decisions about how to go about doing the demo and recording the demo.

Second, the simplest thing you can do is take a simple video recording of the
demo. For example, for a software demo, you can do a `Screen Recording` in case
everything you want to record is present on the screen. You can also record
your audio, explaining the things going on in the screen and what you are
doing. The audio is usually useful in case you need to send the demo over to
someone over email or chat or upload it some server for someone to see. Screen
recording tools are better to record just screens, when compared to using
digital cameras to record your screen, where the resolution of your screen and
the clarity of the content in the video may not be crystal clear. About which
screen recording tools to use - I think there are tons of them that you can
find online. I have used two of them when it comes to screen recording and even
streaming the screen to a service like YouTube / Twitch -

- [OBS Studio - Open Broadcaster Software](https://obsproject.com/). It's Open Source and works on different operating systems. It can do screen recording, streaming of the screen and even record or stream system camera feed and audio feed and has tons of other features! :D

- [Kap](https://getkap.co/). It's Open Source and but works only on Mac. Mainly for screen recording. Also has a few plugins to do something extra with the screen recording :)

For Mac, I have heard that the default built-in QuickTime Player App can also help.

Apart from this, there are also some tools that can record just your terminal
in a very minimal / light format compared to a video. This is useful in case
you are looking to record your terminal if you demo is completely within the
terminal. One of the best tools I have used for terminal recording is -
[asciinema](https://asciinema.org/). It's Open Source and simple to use and can
play the recording completely within the terminal as though you are really
typing and running commands - and anyone can run it. You can also play the
recording in the web with asciinema web player! :D I wrote a complete post
about it - [asciinema to record your terminal](https://karuppiah7890.github.io/blog/posts/asciinema-to-record-your-terminal/).
But I don't think you can record you audio as part of that recording 😅 It's a
very light weight recording of just your terminal and nothing else and the
format is not mp4. It's a special custom very very lightweight format by
asciinema.

Those are the only tools that I have used in terms of recording software demos.

## Conclusion

Recording demos are cool! :D But this does not change the fact that actual
demos are a lot better in many cases, instead of recorded demos. Especially
when it comes to workshops. During workshops, your audience may have lots of
questions and you will need a working demo with which you can experiment and do
things based on the questions of the audience to show live to them about how
something works based on their question. This way, it's easy to clarify things
and answer questions. With live demos, the audience can see live of what's
going on and how a situation / simulation they suggested pans out.

Every demo has it's reason and goals. Preparing for your demos in advance and
also being prepared with a recording as a backup can help. Recording can also
be used to share the demo with people who coudn't attend the demo instead of
just considering it as a backup :)

Like everything in life, you can get better with practice. So practice and
prepare to do your demos in style ;) :D
