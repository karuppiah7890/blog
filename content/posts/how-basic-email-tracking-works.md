---
title: "How basic email tracking works"
date: 2021-04-04T17:44:35+05:30
---

Do you know how email tracking works? In this blog post we will look at some of the basics of email tracking.

A few years ago, I was working on this open source software called Mail for Good by Freecodecamp.

https://github.com/freeCodeCamp/mail-for-good

It was a software used by Freecodecamp and many NGOs for their email campaign management. It was built on top of Amazon Web Services (AWS) Simple Email Service (SES) which costed around $1 for 10,000 emails at that time. I'm not sure what's the cost now though.

Email campaign management softwares are used for multiple things. It's about sending emails to many people for a particular purpose - newsletter, marketing campaign etc, and also managing those email campaigns.

As part of this email campaign management, the mail for good tool provided the usual features that any email campaign management software provides. Some of these include - managing email address lists, getting notifications about how many emails have been sent as part of the list and how many bounced and many more.

There's also analytics around the email campaign which is a pretty important feature. When an organization or individual sends emails to some people, they need to understand if the people they sent it to opened those emails, clicked any links in those emails. And you can get a lot more data like - where the person opened the email - in which country, using their IP. But yeah, the user could have used VPN to hide their actual IP. Anyways, there are also more things like - you can find out what device and browser they used - desktop, mobile, tablet, firefox, chrome, safari etc.

How does all this tracking happen? Let's get into it!! :)

So, first of all, when an email campaign is sent, to track each email, a unique tracker is injected as part of every email. Why? So that each email can be tracked uniquely then you can see the data as a whole to understand how the whole email campaign worked and how effective it was. For example, it's possible that you sent 1000 emails but only ten of them opened it and they didn't click any links in the email. Why does all this matter? Well, it all comes down to the reason for sending the emails.

If you are sending an email campaign to get donations for your NGO, you first want people to read the email. If the email ended up in the spam folder for some reason then they won't even notice it. Or if it ended up in their inbox but they just didn't open it not trusting it and they marked it as spam and blocked it, then also it's a problem. Or they probably just didn't open their mail box for a long time for some reason. At the end of the day, it only matters to you if your email got opened or not. Also, one of the tips I have noticed regarding getting marked as spam is - to avoid getting marked as spam it's good to send emails only to users who subscribed to your newsletters or your email campaigns, unless you are sending cold emails for marketing or for getting donations or similar things. In cold emails, you need to be ready to get marked as spam. Any email service you use would mostly recommend you to send emails only to subscribers. Email servers too take spam seriously, so the service you use to send emails will get affected if their emails, that you sent, get marked as spam, then the origin of the email is at question sometimes, and email servers could get blacklisted too for this reason.

Now, getting back to email tracking.

So, opening of emails matter, because, only if they open and read what you sent, only then they can take action related to what you spoke about in the email. And actions are usually through the same email, where you put a link. This link or links are similar to the call-to-action or call-for-action buttons on websites like "Buy now", "Subscribe now" etc. Ideally these links will lead to some actionable webpage in your website. For example it will lead to a web page where the person can donate money to your NGO. So, clicking on links matter too. After the user clicks on the email link and lands on your web page, it's up to the web page to track the user from there to see if they really donate or had some other difficulty donating money to your NGO, like they didn't know how to pay, what to do, etc.

To track the clicking of links and opening of emails, the general thing that I have seen people do is - they send HTML emails and use some of the features of it. For checking if an email has been opened or not, an `img` HTML tag is embedded with `src` pointing to an API URL. When the email is opened, the mailbox generally tries to load the image if it's from a trusted email ID. When this happens, the image is either loaded, after your system capturing the fact that the email was opened. Or no image is loaded as part of the `img` tag. So it's just a dummy tag for tracking in that case.

Every `src` link value has to be different so that you can hit the API, a GET API and the API can understand who opened the email, and then count the opening of emails. If the link is the same, then you won't know the difference between - if same email was opened multiple times by the same person or multiple people opened the emails sent to them or both. So, it's good to have unique links. If you are interested to capture multiple email openings by the same person, you can capture that too. You basically decide what you want to capture.

Let me show an example

```html
<img src="https://ngo-website.com/email-campaigns/1/users/2000/opening.gif" />
```

Treat the above link as more of a GET API link than a real image source, though you can serve some real images too, like your logo AND also track the email opening

GitHub actually uses this kind of tracking as part of their email notifications. So, when you open a GitHub notification email, GitHub uses an `img` tag and captures the information that you have opened this email. So, when you go to GitHub, under your notifications, the email notification that you opened, it shows that the notification is read / marked as read. This way, you don't have to separately go to GitHub to mark a notification as read if you have already opened the email regarding it.

Tracking email opening is a bit hacky like that, using `img` tag and all. But the idea behind capturing clicking of links is pretty straightforward. This is a technique that Google and many others have been following till date. The idea is similar to that of a URL link shortener, like bit.ly links. The idea is - " redirection ". So, whatever link you want to put, don't put a direct link to the webpage. Instead, put a unique link which redirects the users to that direct link. While redirecting the system will also capture the link click as part of the API.

For example

```html
<a href="https://ngo-website.com/email-campaigns/1/users/2000/links/1">Donate money</a>

<a href="https://ngo-website.com/email-campaigns/1/users/2000/links/2">Checkout our FAQ</a>
```

Again, treat the above links as APIs, which does some background processing to track the link click AND also redirect the user to a final link, which is usually the same for every user that the email is sent to, but maybe has some extra query parameters at the end of the link to track the user again, from the webpage landing.

You can notice this kind of redirection happening even in Google and many other places. Actually it's pretty clever in Google search. When you do Google search, when you hover over links, the browsers show the actual link. It's an actual link. A direct link, which is the same as what you see on the web page text. But when you click on it, I think some javascript code of dynamically changes something and the link changes to a Google link and suddenly you notice yourself loading a Google link which has the actual link in the query parameters for the redirection to happen. So it redirects to the actual link. Google basically captures your link clicking for their search engine and then lets you go to the actual link. And this happens even in Gmail, in Google Chat, when I click links. It probably many other Google services where I haven't noticed it. Many services, if they can track you, it's good to assume they are already tracking you

Some may not consider this kind of email tracking, link click tracking and similar tracking as a good thing and might wanna get rid of this tracking. Others may not care about this tracking and may be okay with it. In any case, it's good to be aware and know how tracking happens in a basic manner and understand it technically too if you are curious about it and then decide if you like it or not and then take actions to not get tracked or at least know when you are getting tracked.

