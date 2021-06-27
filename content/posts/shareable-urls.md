---
title: "Shareable URLs"
date: 2021-06-27T15:07:20+05:30
---

This is a very simple post about the kind of features that users would expect in general when it comes to the ability to share URLs of websites

Let's start with an example. I'm sure many of us would have shared a lot of URLs. A popular example that comes to my mind is - sharing YouTube videos. YouTube allows users to share videos by having a button called "Share", which shows the video's short URL link and also the ability to share the URL on different mediums with ease. It also has the cool feature to share the video URL such that when the user lands on it, the video starts from a particular time :D

Not to mention, one can also simply copy the URL from the URL bar in the browser and share it with anyone :)

From this you can notice a few things - the ability to share the URL in different ways - copy link from URL bar, click "Share" button present as part of the website, and also the ability to share a particular start time of the video

URL - short for Uniform Resource Locator, is to locate resources. From the above example, the YouTube video is a resource, you can probably consider the time of the video as a sub resource, yes? And the website allows you to share the URL with both
- A specific resource - a specific video
- A specific sub resource - a specific start time

That's interesting, isn't it? I mean, if you weren't able to share the specific start time, how would you share it with people in your circle? You would say -

" Hey, see this video https://www.youtube.com/watch?v=ORFWdXl_zJ4 and start watching from 33 seconds "

Now they would have to go to the video and move to the particular time and then see it, or something like that. Not very tedious, it's still something to do - clicks and stuff

But since you can share the specific start time too in YouTube, you can simply say

" Hey, see this video https://youtu.be/ORFWdXl_zJ4?t=34 "

or simply

" https://youtu.be/ORFWdXl_zJ4?t=34 "

Note the `?t=34` in the URL that carries information about the sub resource - the specific start time of the video in seconds

Another simple but very popular example that comes to mind is search engine websites. One can easily search something in the search engine and simply copy the URL in the URL bar and share it with someone. Below is a very simple example

https://duckduckgo.com/?q=wikipedia

Notice that the `?q=wikipedia` is carrying the information about the search query used to do the search in the search engine

With this URL, one can to go to the search engine website with particular search query and it's search results on the web site

In fact this is not true for just search engine websites. Many sites have this shareable URL feature for their search functionality. For example Ecommerce websites - people use so many filters to find products to buy - price filter, brand filter, feature filter, and also sort the results

All this information - filter, sort, is all generally stored in the URL of the website so that the user can simply copy and share it with someone else or just store it for themselves to use later

For example, below is an example from Amazon India website while searching for mouse for a particular set of brands and a particular feature

https://www.amazon.in/s?i=computers&bbn=1375420031&rh=n%3A1375420031%2Cp_n_feature_seven_browse-bin%3A22485336031%2Cp_89%3ADell%7CLogitech&dc&qid=1624784642&rnid=3837712031&ref=sr_nr_p_89_4

You can notice how the filters - Dell and Logitech brands are stored in the URL and I also filtered mouses (or mice? :P) with Bluetooth connectivity feature, which is also encoded in the URL in some way which I didn't try to decode

This is a key feature for users using websites, who would like to share or store their actions in the form of URLs - searches / search queries, search filters. And of course for sharing resources with people in their circles

So, how does a basic user flow look like when it comes to sharing URLs?

First, the user does some action. For example search for something. Let's say they type a search query on a search bar and press enter, the URL in their URL bar changes to show the search query in some form, so that the user can copy the URL for sharing and the webpage content changes to show search results. Same applies for adding any search filters

Other examples of actions are - user goes to a web page by following links in a website, or clicks on an item in the table of contents which leads them to a particular resource

How does the URL look like when it stores a lot of such information about resources and user actions? One can implement this in many ways

One can use the path parameters `https://my-website.com/drama/the-saint/chapter-1/scene-20`

The above URL helps the user to directly click it and land on a specific scene in a specific chapter in a specific drama out of the many dramas present in the website. This is possible only if the URL changes when the user who shared the URL does actions like clicking different links the website by starting from `https://my-website.com/` and then finally landing on a particular web page

This is a classic and simple example of storing resource and sub resource information in the URL

A popular way of storing user action information is using query parameters after `?`. This is pretty standard and noticeable in search engines and many other sites. We saw the DuckDuckGo search engine example `https://duckduckgo.com/?q=wikipedia` where a query parameter, or query param for short, was used. `q` is the query parameter's name and `wikipedia` is the value. For multiple of them, the standard is to use `&` and name-value pairs for query parameters, like this - `https://duckduckgo.com/?q=wikipedia&ia=web` , notice the `ia=web` where `ia` is a query parameter with value `web`

Let's also checkout the popular `#` hash that's used in URLs - this is noticeable in many places too I guess. Many single page applications (SPAs) use these and even simple websites use these. I have seen `#` being used a lot for resource and sub resource identification. A very popular example is using `#` to land on a webpage's particular heading

For example - https://github.com/karuppiah7890/blog#development would take you to my blog's source code and land you on the project's README's development section / heading. Another example is - https://en.wikipedia.org/wiki/DuckDuckGo#Traffic to land you directly on the `Traffic` section on the DuckDuckGo Wikipedia page. If this feature wasn't available, then I would have to share the same by telling someone "Hey, check out this page - https://en.wikipedia.org/wiki/DuckDuckGo and see the Traffic section". Notice the difference?

`#` can also be used to store any information, for example even search and filter information. No one's going to stop you from doing that :) It's just that I haven't seen people use it so much, so I would recommend you to do your research before implementing anything :)

The difference between `?` and `#` is that - when `?` is present in the request, the query parameters (name and value) are sent to the server when the user directly goes to that URL when someone shares it with them

In case of `#`, the parts of the URL after the `#` is not sent to the server and instead the front end (JavaScript) / the browser's built-in feature has to take care of parsing it and processing it

For example `#` part of the URL is a standard thing that browsers parse and look for header tags like `h1`, `h2` etc and see if there's a header with the `id` that's provided after the `#`. Of course the JavaScript code of your website can also parse it and do some actions based on that

No matter what kind of web site one is building, I think shareable URLs is a valuable feature that modern users would expect :) And is a must feature if your website encourages sharing content, for example social media platforms

## Story behind this blog post? A bad UX

I recently used a web app where when I navigated through multiple sections of the website, the URL changed a lot with all the information about the navigation - in some form of bread crumbs. It was a cloud website, so it kind of looked like

`https://cloud-website.com/data-centers/15/network-management/#firewall/management-firewall`

Notice how a combination of path parameters and `#` have been used? The navigation / bread crumb that I followed was -

Data Centers > Data Center 15 > Network Management > Firewall > Management Firewall

Now, I copied this link and opened it up in a new tab in my browser to see if the URL works so that I could share it with my colleagues. The tricky thing that happened was - the web page I landed on was the network management section. The `#firewall/management-firewall` part wasn't parsed by the web page's JavaScript to take me to the Management Firewall directly which is under Firewall Section. This was a bad user experience, as the URL wasn't shareable exactly, at least some parts of the feature didn't work

I decided I'll write a blog post on this to share this experience and my thoughts on it

## Conclusion

It's very valuable for users to be able to refer to inner / sub resources in a page using URLs and being able to share them - sharing URLs referring to a particular heading in a web page, referring to a particular time in a video, referring to a particular action like search action