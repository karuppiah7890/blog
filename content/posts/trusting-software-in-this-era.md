---
title: "Trusting software in this era: Part 1"
date: 2021-05-06T14:05:35+05:30
---

This post is a part 1 as I have lots of small scattered thoughts around this and I have been postponing this post because I wanted to put it all but then I decided to do it iteratively like software development ;)

This post is to provoke some thoughts in people

## Trusting on Open Source

We all use a lot of Open Source. A lot of us trust Open Source, as the source code is open and anyone can read it

Now, if you think about it, most of us directly use the Open Source Software (OSS). When was the last time you read all the source code of the OSS you use?  Do you build the OSS yourself and use it or do you use pre-built software (executables)?

You didn't read all the code huh?! What if I said that's not good? :P

What, you don't know to read the code?! Not a developer? Don't know the language in which the code is written? Some other reason? Well, that's gonna be more tricky!

What, you didn't build the code yourself? What if the people who built the software built it with some other source code or added some extra stuff apart from what's out in the open as part of the OSS project?

Think about it, there are so many Open Source softwares out there. But there are also some malicious people out there. If you go search the Internet, you will find that a lot of OSS have been affected because of malicious hackers who took advantage of vulnerabilities or created vulnerabilities by creating or taking over software libraries or the software itself

No wonder people recommend running vulnerability scanners in your software project to find vulnerabilities in your code and in your dependencies - libraries you depend on, and also understand how to fix them - change the code, upgrade the library you use etc.

Also, did you know that a lot of Open Source software you use is not exactly completely Open Source? For example, take the example of Google Chrome. Google Chrome is "based" on the Chromium Open Source project, but it is not exactly the same as Chromium. Google adds some extra code into Google Chrome apart from the code of Chromium. It could add anything. Here's a sample article from the Internet about the difference between Chrome and Chromium - https://www.howtogeek.com/202825/what%E2%80%99s-the-difference-between-chromium-and-chrome/ . So if you want to use Open Source Chrome, you need to use Chromium. Chrome is not completely Open Source. Only Google knows what they put into Chrome apart from Chromium Open Source code. There are many such Open Source softwares, who's core is Open Source, but people use the pre-built not completely Open Source software. Another popular example is [Visual Studio Code](https://code.visualstudio.com/) vs [VSCodium](https://vscodium.com/)

Even if you are using Chromium, or any other Open Source Software, one thing to remember is - understand that if you are using the pre-built binary, or building it yourself but not reading all the code which can be hundreds of lines of code (LoC) or even thousands and millions of LoC, then you are still in the dark and just using the software. I understand that it's practically impossible to read all the code of a software including the updates which are so frequent, and it's also a bit hard to build some softwares. But I want you to understand that you are using the Open Source software despite not reading the code or even building it, which kind of means that you trust it. Beware about how much you trust this software. For example, are you using an Open Source Password Manager? Do you store your bank's Internet banking and other bank credentials in it? Do you store your credit card and debit card details in it? Imagine the amount of trust you have on the software when you store such sensitive (financially) information. What if the people behind the software misuse the information? Steal from you?!

Be aware of the amount of trust you have on the software you are using

I use many types of open source softwares. Clipboard software is one such. In general the computer's clipboard can only store one thing when you copy stuff, but clipboard softwares help you store more than one thing and you can copy multiple things and it will be stored in the clipboard software. This includes passwords too! Now, do you use Clipboard software? Is it Open Source? Do you trust your clipboard software? What if it reads all the passwords or any content in your clipboard and sends it to the attacker's server through Internet?

So, how much trust do you have on the Open Source software you use? Can you trust it so much that you put in any and all the information in it?

Another thing to note is, even if the software is open source, if it's a service - an online service, where there's a backend server, database etc and it's hosted by someone else and not you, you still have to think if you can trust the people hosting the service, if the source code of the service is open source. What if they changed the code and are hosting it? What if they are misusing the data stored in the service? What if it includes your data?

It's all about Trust! Think out loud about whom or what you are trusting.

## Trusting Closed Source Software

We spoke so much about trusting Open Source software, what about closed source software? Software whose source code is not in the open, and is private / closed. A lot of softwares in this world are closed source. If you think you can trust Open Source Software because it's source code is open, then what about closed source software? And if you think you can't exactly trust Open Source software even if it's source code is open, then what about closed source software?

Just think about the amount of trust you have on closed source software and services you use

## Trusting Software in general

Let's talk about some kinds of software and about the trust in them. It can be closed source or open source software

Are you using an App from Play Store / App Store? It's very possible that it's closed source, but I recommend you to check it out and find if it's closed source or open source. Now, is it a free App? How does the App earn money if it's free for you? Did they inject some virus or malware to do something malicious? Or are they stealing data from you? What data you ask? Your contact numbers, your files (photos, videos etc) and using it all for their benefit? What benefit you ask? Could they show you ads using that data? Could they sell the numbers to telemarketing people? Could they blackmail you with that data? What if you had compromising pictures of yourself or someone in your device? What if they pin some crime on you by using this data and putting you in the wrong place in the wrong time? Or proving that you were really in the wrong place at the wrong time when something bad happened and that you might be the culprit or might be responsible for it or might know something about it though you don't really, but you will be in a pickle (trouble)!

I guess some of the most used free softwares in the world are Google Products. Have you ever paid money to use GMail (Google Mail)? or any other Google Products? Google does have some paid services actually, like Google One subscription, YouTube Red subscription and a few more. But how many of them use it? If someone really can afford those services and needs those services, they might be paying, yeah. No matter if the person pays for the service or not, how much of the user's data is actually used by Google for profit? Google has Ad services that can benefit a lot from user data to show appropriate ads to help their clients acquire paying customers and generate revenue.

Years ago, I remember reading an article which said "Google knows more about you than yourself". Haha. I mean, all those people using Android phones and taking pictures and videos and storing it in Google Photos, do understand that Google has access to it all. If you know this and are okay with it, that's your decision. But ensure that you are aware of this and that it's your decision. I remember seeing Google Photos taking a lot of videos and automatically sensing the content and creating a collage of the videos with a background music and then showing it to me. How creepy is that? I don't know about you, but it was creepy for me!

Please do understand the softwares and services you use.

There are many such examples of free softwares and services. There are so many of them and I think there will be more in the future too.

And sometimes the services you use, could do things that you can't imagine. For example, Grammarly is a service to help you with your writing. Now, if you write some sensitive information to compose an email, and use Grammarly, the information you typed is sent Grammarly servers to process the information to correct grammar issues and complex sentences etc. In this process, they also get to know your information! Did you ever think about how services get your information like this? How can you trust Grammarly to not misuse your information?

The same applies to many tools and services we use. For example, if you want to merge PDFs, which contains sensitive information and if you go and use an online service and upload your PDFs and merge it, the service will merge it and give the merged PDF, but it will also be able to read all the information in the PDFs. Will anyone using such a service think about it?

There are so many more examples - Image Editing softwares, Ad block softwares etc. What if you Ad block software in the browser is tracking all your history? It can because it usually has enough permissions to do so.

We also need to remember that software also has access to the real world hardware and which in turns connect to the real world. For example, your real money is shown digitally in your bank website. Some people can control their home lights and other electronic systems using their phone and an app, as their electronic systems are also connected to the Internet.

Not to mention, your computers also have accessories / peripherals. Computers means - laptops, mobiles, desktops, tablets, raspberry pis and what not - anything that has a "brain" or "processing power". Accessories / peripherals can be a lot - cameras, GPS system, microphones, speakers, and a lot of sensors.

Think about it, is your laptop / computer camera covered using something? Or is it always open? Have you seen people cover their camera using tiny items? Why do they do that?

Have you thought about this - Can someone spy on you? Using your camera? Can someone spy on your system's microphone? Your phone's microphone?

If someone hacks your computer, yup, that's possible. Or if you install non-trusted software into your computer, it's again possible.

One might ask "What's non-trusted software? If I knew it's non-trusted, why would I install it?"

That's a good question. Do you use pirated software? Illegal media content? Music? Movies? Ebooks? During a discussion about pirated Operating Systems, specifically Windows OS, a friend asked a really good question to the person who asked about pirated Windows OS and using it. My friend asked (not exact words) "What's the incentive for people who post this content on the Internet?" He was talking about how the attackers maybe injecting some malware into people's machines using these illegal content?

Just think about it. It's all about trust, again. Do you really trust the people who posted illegal content on the Internet? Can they hack your computer when you play some pirated movie on your computer? Can they hack your computer when you open some pirated ebook PDF on your computer? What would you do if they can? I don't know if it's possible, but I would urge you to check it out if you use pirated / illegal content from the Internet

Along the same lines of malware, do you know what's a ransomware? - https://en.wikipedia.org/wiki/Ransomware

I have heard of some people in my circles being victims of Ransomware. The cases I have heard are usually ones where attackers get access to the complete system of the person and ask for money to give back the access to the person. The complete system is unusable and locked till the money is sent to the attacker. You can go check out more about it on Internet and what's the possibility of getting out of the issue in case it happens. Think about it - the person cannot use their computer, they cannot access their files which has a lot of data. And it's worse if this data includes work data as the person's company will also get affected if it contains work data or if it's a work computer. Not to mention, the attacker will have access to all the data. That's also problematic depending on the kind of data present in the computer. The computer and the data is just a dead duck.

How do you think these people get attacked by ransomware? I can think of non-trusted software as one way.

Now, let's get back to trust on systems and also on people who build these systems.

Have you ever been scammed? Do you know how scams usually work? A lot of scams work by acting genuine of course. This is true in software too. Have you ever received SMS or email regarding your bank or anything and did it look suspicious? I once received such an SMS with a link to a phishing website (fake website) asking me to put in my account details in it to prevent the bank from freezing and closing my bank account, with no mention of the reason for doing so. I went to the website and found an okayish looking website pretending to be my bank's website, asking for all kinds of account and personal details to hack my digital bank account and steal from me. This is just a very simple example. There are so many examples you can find on the Internet.

## Conclusion

This is just part 1. I might write more parts. As for this post, I wanted to ask some questions and provoke some thoughts. As a conclusion, I want you to think about this -

How much of your data is available in digital applications? Are you fine with this kind of exposure? Can it do you any harm? Can it inflict any pain to you? Not to mention, it really depends on what you consider as harm. Someone may say that they are okay with anyone knowing their birthday. They might be okay with the fact that anyone can misuse that information - use it for some forgot password questionnaire or anything. They might be willing to take that risk or they might not even consider it a risk. It's their choice, it's up to them as they own their information and they can decide what works for them.

One thing that I have seen from my experience is, there are a lot of "good" people too in this world, and some "bad" people too, who are trying to steal from or harm others. When these bad people steal or do something malicious, the justice system is able to do only so much. Sometimes, the justice system cannot do much as the attackers work in the loop holes of the justice system. For example, I once heard that someone could not get their stolen money back because they told the scammers their card details and the scammers stole from them. Apparently it was considered more of a mistake. In short, victim blaming.

In a world where there is corruption and bad stuff going on, people resort to victim blaming, why? It has become so hard to catch the culprits, or even impossible. Instead people advice others to be careful. People blame themselves when they get vulnerable and become victims.

It's an unfortunate situation. But that's what's happening. We are asking the victims and vulnerable people to be careful and wary as we are not able to catch the culprits and stop the damage / prevent problems.

If you have ever been scammed or become a victim through software or even otherwise, I recommend you to speak out loud. But yeah, there will be people who laugh at you for being the victim. There will be people who will blame you for being the victim. But if you speak out loud, you can create awareness, if that's something you are willing to do. It might help people to be aware. In case you haven't already solved your problem, people might suggest some solutions and it might even help you to understand how you can solve your problem, with the help of the justice system, insurance system etc
