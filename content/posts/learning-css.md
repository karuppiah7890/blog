---
title: "Learning CSS: A bumpy journey"
date: 2020-09-24T21:35:06+05:30
---

Recently I took up the task of implementing the login feature of our application.
I took it up knowing I know nothing, so that I'll learn it and do it and come
out of it learning something new and with some knowledge on how things work.

I kind of knew that my implementation would involve
[Keycloak](https://www.keycloak.org/), a software we use for Authentication
and Authorization feature in our app. But little did I realize that I would have
to also work with CSS to customize the theme of our login page, instead of
using the default theme of the super cool Keycloak default login page.

So there I was, totally scared about how that's going to pan out - as I have
always run away from anything front end related, especially CSS.

I have done my basic fair bit of HTML, CSS, JavaScript. And quite some React.
Most of them was long ago. But my knowledge was CSS was mostly based on what I
did in college, and a very few professional projects, that too, I learned from
the others more but I still found it hard to wrap my head around CSS.

But I had to learn CSS and do this thing with my partner. Let's see how this
panned out (little secret: It all went well. I'm writing about the past :P)

So, the first thing is - how should the UI look? You need to know how something
looks to design the webpage for it right? In our project, we have our super
cool UI/UX designer who had already created the design for the login page. She
used [Figma](https://www.figma.com/) to do the design and had shared the links
to the design mockups.

So that's sorted - I have the design. Now I needed to know how to even read the
design and understand it and then design the webpage using CSS to match that
design.

I'll give a verbal gist of how the login page looked. It had a background image
for the whole web page, and a big box in the center, inside which there were
logos for branding, and then two input boxes - one for username, one for password
and then a login button to click and login. Nothing very hard or very very
fancy. Just a simple design with some fancy feel. At least that's how I saw it.
For a newbie, you have no idea how wrong I was.

Me and my partner, we started tackling the first thing - the background image. 
I struggled for a few minutes, searching online and learning how to have
background images. After a few minutes, we had found a way to do it. But soon we
realized that on resizing the webpage to smaller sizes, the background image
showed up weirdly. Actually, we were not really aiming for smaller devices, we
were actually aiming for serving tablets and desktops and bigger devices. But we
still tried to see how things looked like, just to see how flexible our creation
was. And it wasn't great - I mean, it wasn't exactly perfect. And then we found
this gem - 

[How To Create a Full Page Image](https://www.w3schools.com/howto/howto_css_full_page.asp)

I just noticed that [w3schools](https://www.w3schools.com) has lot of `How To`
guides in their website. Something for me to checkout later.

I had played with their demo and understood to some extent what was missing
in our case and fixed our background image.

The next thing to tackle was positioning our big box inside which everything
was present - logos, input boxes and submit button. This was a big white box.

I checked out the design, it had pixel specifications for the big white box. At
some point I realized that I can't have exact pixels for the design - and that
it wouldn't make sense for the webpage to be used in different devices. And I
didn't even know much about pixels, especially in CSS 🤦. I still need to learn
about pixels and sizes. Now, with this thought, I decided to use percentages for
mentioning sizes - for widths, heights, any distance.

Now I had to put this big white box in the center of the webpage. I realized I
need to position it. I copied some `position` CSS code from the figma design,
but it didn't really workout. Yeah, figma provides CSS code and some really
good spec information, I think the latter is necessary for design :)

I realized I didn't even know about `position` in CSS. So, I decided to learn it.
I saw this 

[CSS Layout - The position Property](https://www.w3schools.com/Css/css_positioning.asp)

Yet again w3schools to the rescue! It was a really good way of explaining things.

I quickly started positioning the box based on the design, and using percentages
and using `position: relative` and `position: absolute` a lot, to position things.
For size - the design had sizes in px - I did calculations and found ratios with
respect to another element - parent element or the screen and found the
percentage. For example, `54px` width of an element inside a `532px` width
element -

`54 / 532 = 0.1015037594`

That's `10.15%` in short, so I used that!

It was all working out well. I even tried to resize my browser to simply check
out how flexible things are and it looked good actually. Later, my partner
tried things and said it's breaking for him and he recommended for us to fix
it. I saw this on his computer and was confused. It's still a mystery to me.
Anyways, after some of this design, it was still incomplete as there were
quite some things to do, but we showed this to our designer and also another
colleague to get some feedback. They gave feedbacks around the design. One of
them was - make the big box square, no matter the screen size, it should always
be square. Another was - usage of `rem` for sizes. Finally, they mentioned
flexbox for layout.

I had already started checking out a bit about flexbox and CSS grid before this
feedback - mostly because I realized that things are breaking in my partner's
browser for some reason, even though I wasn't sure what.

I had heard about flexbox and CSS grid a lot before. I have even tried some
basics of flexbox, and very little CSS grid too I think, long long ago. I used
to keep roaming around the web trying out unnecessary things when it wasn't
needed for me to know them :P

I was checking out about flexbox and CSS grid - and boy oh boy, I realized it
was a big topic and also found out about the browser support for these cool
things and noticed that it's something to consider given we want to support
a good amount of browsers, latest versions in the major browsers.

After some digging, I found out that both flexbox and CSS grid have their use
cases and that no one's restricting you to use one or the other or even both
of it. Preceding this, I only remember reading flexbox is 1D and CSS grid is 2D.
After my recent reading, I realized that only talking about that isn't good
enough.

Some main links I read are -

[Flexbox vs. CSS Grid: Which Should You Use and When?](https://webdesign.tutsplus.com/articles/flexbox-vs-css-grid-which-should-you-use--cms-30184)

[Use Cases For Flexbox](https://www.smashingmagazine.com/2018/10/flexbox-use-cases/)

I skimmed through few more. Let's just say I agreed to my colleague's feedback
based on what I had read and went ahead with flexbox for the login page - as we
were also using flexbox in our React app - which is separate from this standalone
customization of Keycloak Login page. We were also using `rem` in the React app.

I quickly started doing my overview and reading of rem, and em and how to use
it and also noticed the code in our React app.

This was quite a good article on `rem` -

[Rem in CSS: Understanding and Using rem Units](https://www.sitepoint.com/understanding-and-using-rem-units-in-css/)

It also has references to so many other links :)

With this reading and our React codebase, I went ahead and used `rem` for almost
everything in our CSS and also started using flexbox - in a very basic manner.

While doing this, I also checked out [Sass](https://sass-lang.com/). Our CSS
was too simple - we didn't have to use Sass / Scss - I was using only very very
basic features in it. But I felt it will help to easily read the code and
change it too and have it's super power always :P Now, how did I end up using
Sass you may ask? Well, our React codebase uses Sass, also, I noticed a lot of the
code examples online used Sass and I was having trouble trying them out by
finding the converted CSS :P

I have actually heard very little about Sass / Scss. I just knew it had some
features and has to be converted to CSS for usage. "preprocessor" is what I had
heard before. I didn't even know Scss and Sass are related closely. I found
out only from the Sass website about how I can write `.scss` files and it's
still the same Sass tool being used.

Just imagine - how I went from 0 to so much, by just jumping into something I
didn't know, something I wasn't comfortable in, and by just reading about things
online and from our existing React app codebase for styling.

With all this, I had slowly started getting some hang of CSS. I know there's
still tons. I mean, people have been doing CSS for years, and I just started
a few days ago to properly learn it. Typography, lengths, best practices,
complex designing, large scale web apps with lots of designing, maintaining
CSS in such big codebases, so many things.

I quickly realized how Sass helps, even though I always saw it as some complex
thing. It's very helpful in our React codebase.

I'm yet to learn more about the features of Sass, or even CSS, which seems to
have evolved a lot over the years, with many fancy features out of the box,
but yeah, it has been too cool!

Later, I had also started learning a bit about fonts, and how to import new
fonts into webpages and use them, and size them, and even about spaces between
letter, height of lines. A lot of small things. And also how to do development
easily with the developer tools, to check these features and if my design is
correct.

Finally I started learning about how to do small transitions for the placeholder
for our username and password input box, to get an animation effect. In
technical terms, it was actually an effect for the input box's label, and not
really the placeholder, but for the layman user, it will look like a small
animation on the placeholder.

I think I might write separate blog posts about each of these small things and
try to dig in a lot into each of the concepts and explain in simple terms how they
all worked.

I think, in the end, I conquered my fear a bit, and came out well, learning a
thing or two. It took time, but it was possible. There's more to learn as always
:P And more fears to conquer. :)

Do put in your questions and comments below :)
