---
title: "Drawing Tools Part 1: Excalidraw"
date: 2020-11-21T14:23:37+05:30
---

### TLDR;

Try [Excalidraw](https://www.excalidraw.com/) an Open Source Online drawing tool
which has enough features for most of your basic needs! It also supports live
collaboration for people working remotely! :)

### Video Version

Before going into the post, if you prefer videos over blogs, you can checkout
this YouTube video, which has the same information as this blog post.

{{<youtube HZRayUIqhAA>}}

This blog post will be mostly the text version of the video :) There might be a
thing or two extra in this blog post.

### Longer Version

#### Introduction

If you are in the tech industry like me and you work in a software team, a lot
of times you might have to draw some technical diagrams to explain some
concept. Diagrams are a really good way to explain something. Many people in
the world are visualizers and can understand better when they see something
visually and not just hear something when someone explains with just words.

Technical diagrams have always been popular. I think you might have drawn a few
too. Now with the more of remote work due to corona virus, people have to
communicate digitally a lot and online tech diagrams have become very important
at these times, so that you can draw online during a meeting for a technical
discussion. Also digital technical diagrams are however needed for digital
stuff like blogs/articles and also for any sort of digital artifact, say
documentation. Or to even print it on a paper. And usually digital ones are
pretty easy to save and modify too and you can have access to all versions too
having a version history. You get the drill.

Another cool thing that has been enhanced in a lot of tools now a days is -
live collaboration. Since more people are going remote now and working away from
office. Many remote collaboration tools and features existed before too, but you
can see more rising in the recent times.

I'm doing a series a videos and blogs about tech diagrams. YouTube playlist is
[Drawing Tools for Tech Diagrams](https://www.youtube.com/playlist?list=PL6xKBQ9SsrsYK5TtnEOiDpwoYLXLBt9re).
This is the first blog in this series.

Let's dive into the first tool in this series. We are going to be looking at the
trending tool called [Excalidraw](https://www.excalidraw.com/) in this blog
post.

#### Excalidraw

[Excalidraw](https://www.excalidraw.com/) is an Open Source drawing tool. You
can find their source code on [GitHub](https://github.com/excalidraw/excalidraw)

The tool is pretty intuitive to use and is pretty simple. I feel it has enough
basic features that most people need for simple drawings. But I guess your
definition of simple and mine may vary, so we will see what features it has to
offer and you can decide for yourself if it's good enough for your use case :)

We'll get into each of the features in different sections

### Open Source

I would consider this as a feature. As I already mentioned,
[Excalidraw](<(https://www.excalidraw.com/)>) is Open Source with their source
code on [GitHub](https://github.com/excalidraw/excalidraw)

### Hand-drawn diagrams

This is an attractive feature in Excalidraw. Anything that you draw with it, by
default it draws it as though it's a hand-draw drawing. The lines are a bit
sloppy. This gives a realistic effect, similar to how people draw on real
whiteboards - usually most people can't draw perfect straight lines and perfect
shapes. This is also a good reason to call themselves as Virtual Whiteboards.
This is a feature that I love. But if you are not into sketching hand-drawn like
diagrams, you can still turn off this feature, it's not forced on the users to
only be able to draw hand-drawn like diagrams.

### Basic Shapes

You can draw basic shapes like Rectangle, Diamond, Ellipse, Arrows, Lines.

### Free hand drawing

If basic shapes are not enough for you and you want to draw something complex or
different, there's a free hand drawing option. You could use that too ;)

### Infinite Canvas

There's an infinite canvas. You can keep scrolling up, down, left and right.
There's also a nice "Switch back to content" button if you go to an empty space
where there's none of your content / drawings.

### Live Collaboration

It's pretty easy to start off a live collaboration session. You can share the
room link to others and they can join the session room. You also don't have to
worry about the Excalidraw servers reading your data. There's zero privacy
breach as Excalidraw uses End to End (E2E) Encryption. You can read it in their
[End-to-End Encryption in the Browser blog post](https://blog.excalidraw.com/end-to-end-encryption/)

> Note: If someone gets access to the live collaboration URL, then they can join
> your room and access your drawings. So, be sure to keep your collaboration URL
> safe. I believe that it's a hard to guess URL so I wouldn't worry much about
> it. You can read more over here in the [GitHub issue](https://github.com/excalidraw/excalidraw/issues/2408)

### Component Library

You can draw stuff and then select them and then add them as a component in the
component library. You can also export the component library and also load
an exported component library. I feel this is a very powerful feature! Using
this you can easily reuse parts of your drawings across different drawings and
also within the same drawing. Usually for reusing within the same drawing people
just copy paste, which is another simple way. This is more powerful to share and
also reuse parts of drawings (components) in different drawings. Do try out this
feature! :)

### Exporting and Sharing

This is a pretty important and basic feature for any drawing tool - ability to
export and download the drawing you drew.

Excalidraw has options to export as SVG, PNG. There's also an option to share
the drawing using a link :)

> Note: If someone gets access to the share link, then they can get access your
> drawings. So, be sure to keep your share link safe. I believe that it's a
> hard to guess link so I wouldn't worry much about it. You can read more over
> here in the [GitHub issue](https://github.com/excalidraw/excalidraw/issues/2408)

### Exporting and Importing full scene in Excalidraw format

Apart from saving as a simple image, you can also export in Excalidraw's format
which is a special JSON. Why would you do this? Well, if you export as JSON,
then you can import the JSON again into Excalidraw later ;) That's the
difference between using PNG, SVG vs Excalidraw JSON. With Excalidraw JSON, you
can import the JSON and continue drawing on top of it, and again export it as
JSON too :D

### Internationalization

Excalidraw has support for many languages - this is usually about what language
the help, messages, guides and other information are shown in. They are also
getting community help to do the translations. If you are into translations and
helping with Internationalization for an Open Source project, this is one
project you can checkout. There are many Open Source projects that look for help
when it comes to translations :)

### Copy as SVG and PNG into clipboard

You can select a drawing and right click and choose an option to copy the
drawing as SVG or PNG into your clipboard! :D

Note: Copy as PNG into clipboard feature works only in Chrome. It is not present
in Firefox

### Light and Dark mode

Excalidraw supports both Light and Dark mode ;)

### Basic features of many drawing tools

Apart from the above features, Excalidraw has many basic features that many
drawing tools provide and is kind of like the basic expectation from users

#### Selection and movement of drawings

Excalidraw has a selection tool and the ability to move drawings with ease

#### Rotation of drawings

You can also rotate drawings at any degree after you select the drawing

#### Zooming in and out

There's option to zoom in and zoom out so that you can look at the minute
details and also look at the big picture

#### Grid Mode

You can right click and toggle grid mode if you like to have a grid when doing
your drawings

#### Copy, Paste

You can easily copy and paste diagrams that you draw. You can right click on
your drawings and see options to copy. You can also right click and see an
option for pasting. :)

#### Keyboard Shortcuts

Excalidraw has lot of keyboard shortcuts to do many actions! Many standard
keyboard shortcuts are present too. For example Copy, Paste shortcut. Move
action keyboard shortcut using arrows. Undo, Redo keyboard shortcut. Shortcuts
to choose shapes to draw. Shortcuts to zoom, duplicate and more! :) Almost most
features have a keyboard shortcut :) There's a small keyboard icon on the bottom
right that you can click to checkout the different keyboard shortcuts present

#### Options for shapes and text

When you draw shapes or write text, there are lot of options that you can tweak.
If you choose your drawing or text, you can notice a lot of options on the left.
There are options for color - the outline / stroke. There are some 15 standard
colors, but you can also type any color yourself manually using RGB hex color
value :)

For drawings, there's options related to background color, and then the way the
filling should look like, if there's a background color. For example if you want
to color a rectangle black - it can be full black filling. Or you can have some
lines far apart, to fill the rectangle, which is called Hachure. There's
parallel lines, and then parallel and crossing lines. Do try it out :) And then
there's the usual solid filling to fill the shape completely.

Then there's stroke style - it can be a continuous line, or a dashed or dotted
line.

There's also the sloppy feeling / sloppiness of lines. This is what gives the
hand-drawn drawing feeling. If it's not at all sloppy, then it's a straight
perfect line. Then there's two levels of sloppiness.

You can also configure how the edges look like - rounded or sharp.

Then there's opacity of the shape and text.

#### Layers

Excalidraw has the concept of layers too. You can check it out too! It will be
pretty useful when you have overlapping shapes and drawings
