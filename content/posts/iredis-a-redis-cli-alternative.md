---
title: "iredis: a redis-cli alternative"
date: 2021-10-10T17:02:30+05:30
draft: true
---

I recently noticed this `redis-cli` alternative called `iredis` and started trying it out today. I figured I'll share a few things about it

First, the CLI is open source, the project is here - https://github.com/laixintao/iredis with a slick website https://iredis.io

As the project describes it, `iredis` is an interactive Redis CLI client, and it also has a slight fancy touch to it. Usually my first thought whenever I see alternatives is "But why? Why not just keep it simple?" and this is especially true when I see fancy alternatives. But let's see what `iredis` has to offer and what are some small things I would change about it if I could

If you check the https://iredis.io , you should be able to see many of the cool features it has to offer. And the repo also has a list of features - https://github.com/laixintao/iredis#features

`iredis` has some interesting selling points. Let's see some of them

## Reverse command search

For someone like me who is so used to using `bash` shell, I sometimes use some keyboard shortcuts without thinking. For example, I would do `Ctrl + r` to do a reverse search of commands but only some shells support it, and I have seen that database shells don't support it. The same is true for `redis-cli`. In such cases where one cannot do `Ctrl + r`, you just have to press up arrow multiple times and see the commands you typed a few moments ago and choose the one you want to run again, or just type the whole command again, or maybe keep the commands handy in some notes / markdown file and copy paste it when it's needed. Keeping databases commands in notes is something I have seen many people do. `iredis` solves this by providing the `Ctrl + r` keyboard shortcut and allows you to do reverse search of your commands within the iredis shell. That's pretty cool actually! I want to talk about a related thought here. A few years ago, I stopped using the basic default `Ctrl + r` reverse search of commands and started using [`fzf`](https://github.com/junegunn/fzf) which has [keybinding for `Ctrl + r`](https://github.com/junegunn/fzf#key-bindings-for-command-line). I use it a lot now! I can't imagine going back to the default reverse search for commands. `iredis` provides a simple reverse command search like `bash`, and I think it would be cool to use `fzf` like integrations there but it's a bit too much to ask for I guess 😅 Also, if one is using `redis-cli` or any Redis CLI client and wants to have a similar feature for reverse search for commands, then they have to use the CLI in non-interactive mode, like, use `redis-cli PING`, `redis-cli GET users:1000` and then simply use the bash `Ctrl + r` to find the `redis-cli` command along with the Redis command and one can use `fzf` too and easily use a similar feature. But yeah, if you a workflow like that, you lose the interactivity of `iredis`, which we will come to next

## Exiting from iredis shell

`iredis` is interactive in some nice ways, even the `Ctrl + r` is an interactive feature only, within the `iredis` shell. Other interactive features include - showing how to exit out of the shell at the bottom of the screen, so that people are not confused like it's `vim` not know how to get out of the `iredis` shell. To get out of an `iredis` shell, people have to use `Ctrl + d`, this is similar to logging out and exiting `bash` shell. In `redis-cli`, if one uses `Ctrl + c` or `Ctrl + d`, it would exit the shell. In `bash` though, `Ctrl + c` only stops / interrupts / terminates the execution of the command being written, and the same happens in `iredis` too, which is pretty intutive for those very used to using `bash` and shells with behaviors similar to `bash`

When exiting the `iredis` shell, it shows a nice `Goodbye!` message too and greys out the last `iredis` shell prompt ;) All pretty interactive to show that the command is done

## Simple Connection details

Every time you connect to the Redis server using `iredis`, the first thing it shows is the `iredis` version, to show the Redis client CLI version and the Redis server version too. This is a very simple and important feature for a client - to show all the versions of the software installed. It is true that you can get this detail by using `redis-cli` too, using the Redis command `INFO server` and checking the `redis_version` field's value, but still, that's all done automatically here and shown while connecting itself

## Auto completion

This is pretty cool!

## Help and docs

Full docs from [redis-doc](https://github.com/redis/redis-doc) repo which is more than what `redis-cli` provides when running `HELP`

The `iredis` software seems to use the [redis-doc](https://github.com/redis/redis-doc) repo's reference through git submodules in it's code - https://github.com/laixintao/iredis/blob/master/.gitmodules and I think that's how they import all the code

## Extra iredis commands

Apart from Redis commands, there are some `iredis` commands to in the shell! For example, `peek`!

## Install without Python

This is pretty important. CLI installation should be pretty easy. If I had to change one thing about the CLI, maybe I would have chosen to write in Golang, Rust or some compiled language where I can get one binary to distribute. `iredis` still has done a pretty good job, one can install without Python too very easily, with the following steps.

Note: The below seems to be for Linux only

```bash
wget  https://github.com/laixintao/iredis/releases/latest/download/iredis.tar.gz  && tar -xzf iredis.tar.gz  && ./iredis
```

The tar ball is also only around ~35 MB, which is pretty reasonable I guess

## Preventing dangerous commands

This is an interesting preventive measure. It's important to not run dangerous / disruptive / heavy operation commands like `DEL`, `KEYS *`. For this reason, `iredis` provides a way to ensure that there's a preventive measure by asking the user for confirmation if they want to run such a command, that too after providing details as to what harm the command can do

```bash
blog $ iredis
iredis  1.9.4 (Python 3.10.0)
redis-server  6.2.6 
Home:   https://iredis.io
Issues: https://iredis.io/issues
127.0.0.1:6379> keys *
KEYS will hang redis server, use SCAN instead.
Do you want to proceed? (y/n): n
Canceled!
127.0.0.1:6379> del food
DEL will delete keys, it may cause high latency when the value is big.
Do you want to proceed? (y/n): 
DEL will delete keys, it may cause high latency when the value is big.
Do you want to proceed? (y/n): 
DEL will delete keys, it may cause high latency when the value is big.
Do you want to proceed? (y/n): l
Error: l is not a valid boolean
DEL will delete keys, it may cause high latency when the value is big.
Do you want to proceed? (y/n): n
Canceled!
127.0.0.1:6379>
```

As an ops person, I think a good alternative is to prevent such commands in the backend in the Redis server itself, by disabling such commands for all users except a few. Disruptive commands like `DEL` can be temporarily enabled when needed, say during some production issue. The disabling of commands can be taken care by using Redis [ACL](https://redis.io/topics/acl) in the latest versions (> 6) of Redis as of this writing


