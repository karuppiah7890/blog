---
title: "Tools tools everywhere"
date: 2021-08-19T20:07:30+05:30
---

I love the current era of software for some reasons. It's making lives easier at times. For example - there's tons of tooling out there to help with a lot of things

People writing new products and software and tech are also considering things like upgradability, maintenance, migration etc

For example, if you are migrating a software from one major version (semantic versioning) to another major version, and also doing cool crazy stuff like doing skip upgrades like v3.x.y to v6.a.b, these days developers provide tooling to help with this!

Let me give some examples so that it's clear. I think the first time I heard about upgrade tools was when someone mentioned about `terraform` CLI's upgrade command! I haven't personally used it, but I have seen it and it seemed pretty cool! Here's the doc for it -

https://www.terraform.io/upgrade-guides/0-12.html#pre-upgrade-checklist - Terraform introduced a `terraform 0.12checklist` command for checking pre upgrade checklist !

https://www.terraform.io/upgrade-guides/0-12.html#upgrading-terraform-configuration - Terraform introduced a `terraform 0.12upgrade` to upgrade Terraform configuration files!

I think this is pretty cool stuff! Another example that comes to mind is Helm v2 to v3 migration. Helm maintainers created a pluin called `2to3` to help users migrate from Helm v2 to Helm v3 which was a big step and had a lot of major changes! The repo is here https://github.com/helm/helm-2to3 and the command looked like `helm 2to3 move config`

There are many softwares today that bring with them the feature to upgrade to the next version or the next big thing in some way or the other. For example, all the GUI tools I use on my Mac, it has a `Check for updates` option usually, which checks for updates and shows if there are updates and all of them also automatically update the software when told to do so in a few clicks or just a single click. For example, VS Code Text Editor, Sublime Text Editor, Docker for Desktop Mac, Firefox browser, Chrome browser, Alfred tool. The CLI tools also show messages when there are new releases available, to show that the users can upgrade. Of course package managers also help with this kind of upgrades, for example `brew update` and `brew outdated` helps to show the outdated installed packages in my local that I can upgrade, but yeah, it's possible that the package is updated but brew package repository is not, but I'm okay with that slow speed at times, as long as I get automatic updates and my installations are managed automatically.

I think it's cool that tools are being used really really well to enhance the lives of software developers. I love it when tools can do the work I need to do :P so that I can do something else! :P

If a tool can do something, it's better for it to do it, especially when it comes to some mundane tasks, and also when it comes to some really complex tasks, which when done by a human is also prone to human error when done manually. I'm not saying automated programs are perfect, because humans write those and there can be bugs there too :P But still, one can write tests and what not and ensure program works well and the program can do heavy lifting tasks and also mundane boring stuff!

Another example I want to show finally that I noticed today on Twitter was - caddy server supporting nginx configuration. Caddy server simply put is a web server, a popular one. Nginx, simply put, is a web server too, a very popular one in fact. And caddy server has a project to support people using nginx and nginx configuration, to support them and help them migrate to caddy server. Project link - https://github.com/caddyserver/nginx-adapter

I think it's cool that softwares provide migration paths from a different software to their software. It's a pretty standard thing, but still. Of course this would benefit the software because others would be willing to try the new software if it's easy to migrate and try it out and give it a spin. Not sure if there are softwares out there that would also support the migration path from their software to a different software, because that's like helping people move out, but that's a cool thing too! Maybe not a necessary thing, as the other software can take care of that - take care of the onboarding to their product which also includes migrating from different products to their product

Anyways, I just wanted to call out that automation is SO COOL and I'm in love with the different kinds of cool stuff that people think about! Which reminds me, I too thought about a small and simple thing as part of the Helm v2 to Helm v3 migration :P It was a simple tool to help people use a specific feature of Helm v3 when moving from Helm v2. https://github.com/karuppiah7890/helm-schema-gen/ It's a very simple and small tool with very specific features. I don't maintain it now, as it was supposed to be just a small time frame project, a one time thing and that's it. Like a side project for fun and also a very small one. I don't think I'm into maintaining software for years or decades :P For this reason I like really small and simple tools ;)

So yup, that's a wrap ! If you have an automation idea, go out there, make the world better and easier ;) The software world ;)

Let me know your thoughts about tools in the comments! Do mention what kind of tools you have loved in the past, what among them do you still love and tools that you love currently! :D
