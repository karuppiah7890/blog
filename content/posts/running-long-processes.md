---
title: "Notifications for running Long Processes in Terminal"
date: 2021-02-26T10:42:35+05:30
draft: true
---

Example: Tests, Build / Compile

How to know if it's been done? For example in the terminal when you run a
command that runs for more than 5 minutes.

You can go and do push ups. But it would be good to get a notification once it's
done instead of you continuously checking it's done. Also, if you need to do
some task after the process is done, then it's key to get this notification.
Some tools have started to provide this feature by default. For example Jest
test framework has it. You just need to do

```bash
$ jest --notify
```

There are also command line tools to help you with just notifications.
terminal-notifier is one such tool ! :D

https://github.com/julienXX/terminal-notifier

It's written in ruby. It works on my Mac very smoothly. It's only for Mac
though and for particular versions of Mac.

Some cross platform libraries for the same can be found in npm as Node.js
packages

https://www.npmjs.com/search?q=terminal%20notification

For example

https://www.npmjs.com/package/node-notifier

https://www.npmjs.com/package/node-notifier-cli
