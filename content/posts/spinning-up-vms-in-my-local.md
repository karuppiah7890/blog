---
title: "Spinning up VMs in my local"
date: 2020-03-13T08:42:19+05:30
---

This post is about how I spin up VMs in my local machine. This is considering I have already chosen to run
something in a VM and not in a container like Docker container or any other tech like unikernels and others
that I don't know about :p

Now, back to VMs. Now, usually I used to use Virtual Box to run VMs in my local. It's open source and I can
run it in my Mac. I still use it. I have run Linux based VMs, and even Windows. Recently I have also
started using this software called multipass. multipass can help you spin up ubuntu VMs. Seems to be a good
software by Canonical. multipass makes it easy to spin up Ubuntu VMs very easily from the command line and
ssh into it too. With respect to VirtualBox, you can use an ISO image and install an OS in the VM and then
use it. Another smooth way would be to use Vagrant! Vagrant is a layer on top of VirtualBox. Vagrant helps
you run VMs using VirtualBox, Parallels and more virtualization softwares available out there. Vagrant also
provides a command line interface, so you can do everything from your command line. And you can also easily
spin up VMs - you don't have to worry about getting ISO images and installing and so on. Below is a demo to
show how you can spin up a Debian VM and also mount a directory in your local into the VM with vagrant and a
demo for multipass too

Vagrant demo (~23 mins 🙈)
{{<asciinema 309770>}}

multipass demo (~10 mins)
{{<asciinema 309771>}}

How do you spin up VMs in your local? Let me know in the comments! :)

Some links for references and to read more about the stuff mentioned in this post:
https://www.virtualbox.org/
https://www.vagrantup.com/
https://app.vagrantup.com/boxes/search
https://multipass.run/

Source code:
https://github.com/hashicorp/vagrant
https://github.com/canonical/multipass
https://www.virtualbox.org/browser
