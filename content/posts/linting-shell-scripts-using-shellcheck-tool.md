---
title: "An introduction to linting shell scripts using shellcheck tool"
date: 2021-05-22T17:12:05+05:30
---

## TLDR;

[`shellcheck`](https://www.shellcheck.net/) is a open source linter for your shell (sh/bash/dash/ksh) scripts! The official website calls it as - "a static analysis tool for shell scripts". A small example usage -

```bash
$ cat list-special-files.sh
#!/bin/bash

dir=$1

for file in "$dir/special-*"; do
  echo $file
done

$ shellcheck list-special-files.sh

In list-special-files.sh line 5:
for file in "$dir/special-*"; do
            ^--------------^ SC2066: Since you double quoted this, it will not word split, and the loop will only run once.


In list-special-files.sh line 6:
  echo $file
       ^---^ SC2086: Double quote to prevent globbing and word splitting.

Did you mean:
  echo "$file"

For more information:
  https://www.shellcheck.net/wiki/SC2066 -- Since you double quoted this, it ...
  https://www.shellcheck.net/wiki/SC2086 -- Double quote to prevent globbing ...
```

## Longer version

### Discovery

Recently me and my colleague started writing some bash scripts to automate the running of some commands. We were doing this to do initial automated setup for running automated End to End (E2E) tests

When I was writing these scripts, I was just running them manually to check if all is working good and then finally raised a Pull Request for review. During the review, one of the comments requested me to run some checks manually as the automated checks (Continuous Integration) wasn't working for some reason

When I ran the checks using `make check` (Makefile), I found out some issues in the bash script I had written! I noticed that this was done using `shellcheck` tool

I had previously noticed `shellcheck` as a plugin in IntelliJ IDEA IDE but never used it in many projects and when I did enable it and use it, I never noticed any useful errors from it 😅

Also, to add to this, I assumed `shellcheck` was some proprietary closed source plugin by IntelliJ IDEA IDE 😅 And never cared to look it up online

Once I saw `shellcheck` run as part of `make check`, I kind of realized that anyone could use `shellcheck` outside of IntelliJ IDEA IDE and started checking it out! 😁

`shellcheck` is an open source tool! 😁

shellcheck website - https://www.shellcheck.net

### Why shellcheck ?

I have been telling you about the whole story of how I discovered shellcheck, now let's see why we need it

Like any other linter, shellcheck linter helps with finding issues in code - shell script code

I keep calling `shellcheck` as a linter. The official docs and website mention "find bugs in your shell scripts" or "a static analysis tool for shell scripts". A linter is also kind of defined like that so I just call it as a linter. Wikipedia for linter and lint

https://en.wikipedia.org/wiki/Linter

https://en.wikipedia.org/wiki/Lint_(software)

Now let's see some code examples to understand how `shellcheck` looks like in action and how it helps developers writing shell scripts!

Below is an example code -

`list-special-files.sh`

```bash
#!/bin/bash

dir=$1

for file in $dir/special-*; do
  echo $file
done
```

I'm trying to list some special files or even directories in a directory, which have the prefix `special-`. Let's look at how it works

```bash
$ vi list-special-files.sh
$ chmod +x list-special-files.sh

$ mkdir special-directory
$ cd special-directory/
$ touch special-pluginA
$ touch special-fileB
$ cd ..
$ ./list-special-files.sh special-directory
special-directory/special-fileB
special-directory/special-pluginA
```

Though this script worked in the above case, it may have problems in some cases, let's see one example where it can have problems

```bash
$ mkdir "cool directory"
$ cd "cool directory"
$ touch meh
$ touch special-pluginA
$ touch special-fileB
$ cd ..
$ ./list-special-files.sh "cool directory"
cool
directory/special-*
```

Notice how the output is not what we expected? The `cool directory` also has the same `special-pluginA` and `special-fileB` files as `special-directory`, but the output is different

But it's hard to keep checking different outputs for different inputs manually. If you prefer to write tests, sure, go ahead :) I think that's also a good way to automatically verify and ensure that your code works in different possible cases. Or if you think you can write this same code in a different programming language and then write tests for that, sure! But in this case, I'm going to check if `shellcheck` can help and provide some insight as to how good the code is

```bash
$ shellcheck list-special-files.sh

In list-special-files.sh line 5:
for file in $dir/special-*; do
            ^--^ SC2231: Quote expansions in this for loop glob to prevent wordsplitting, e.g. "$dir"/*.txt .


In list-special-files.sh line 6:
  echo $file
       ^---^ SC2086: Double quote to prevent globbing and word splitting.

Did you mean:
  echo "$file"

For more information:
  https://www.shellcheck.net/wiki/SC2086 -- Double quote to prevent globbing ...
  https://www.shellcheck.net/wiki/SC2231 -- Quote expansions in this for loop...
```

Cool right? 😁 If someone looked at the script code, at first sight it would have looked good. But as we saw, it has problems in some cases. And `shellcheck` gives some errors and says that there might be some problems in this code.

Notice how I said "might be". I believe that any linting or analysis tool gives errors / warnings too, but it's up to the author to check it and decide how to act upon it. I have also noticed people disable linting errors or warnings in some cases, for various reasons.

Now let's look at what the errors in the `shellcheck` output mean.

I see two codes and links for the two codes, and also a short description, and even a suggestion on how to fix it

First code is - SC2231 - https://github.com/koalaman/shellcheck/wiki/SC2231

This one tells me that it's better to use double quotes for variables in such a case where I'm looping through a glob. This is to prevent splitting of words, for example when the variable contains a space. And this also handles other special characters in the word

Notice how `shellcheck` shows exactly what parts of the code I need to put double quotes, using arrows / carrot symbol (^) and hyphen (-)

```bash
for file in $dir/special-*; do
            ^--^ SC2231: Quote expansions in this for loop glob to prevent wordsplitting, e.g. "$dir"/*.txt .
```

The other code is  - SC2086 - https://github.com/koalaman/shellcheck/wiki/SC2086

This mentions how a double quote can again avoid globbing and word splitting and also shows other examples apart from usage in `echo`

We never noticed any issues with the `echo`, right? Let's look at how our current code can be problematic when using `echo $file`

```bash
$ cd special-directory/
$ touch "special-*"
$ ls -al
total 0
drwxr-xr-x    5 karuppiahn  staff    160 May 22 16:36 .
drwxr-xr-x+ 319 karuppiahn  staff  10208 May 22 16:31 ..
-rw-r--r--    1 karuppiahn  staff      0 May 22 16:36 special-*
-rw-r--r--    1 karuppiahn  staff      0 May 22 16:35 special-fileB
-rw-r--r--    1 karuppiahn  staff      0 May 22 16:35 special-pluginA
$ cd ..
$ ./list-special-files.sh special-directory
special-directory/special-* special-directory/special-fileB special-directory/special-pluginA
special-directory/special-fileB
special-directory/special-pluginA
```

Notice how it uses the glob pattern and shows all files because the file name has a special character `*` which is used for glob / as wildcard character

Now let's fix all these issues in our code and try the `shellcheck` tool again and also do some manual testing with the same old examples

`list-special-files.sh`

```bash
#!/bin/bash

dir=$1

for file in "$dir"/special-*; do
  echo "$file"
done
```

Let's run `shellcheck` now

```bash
$ shellcheck list-special-files.sh
$ echo $?
0
```

Cool !! No errors ! 😁

Let's do some manual testing the `bash` shell

```bash
$ ./list-special-files.sh "cool directory"
cool directory/special-fileB
cool directory/special-pluginA

$ ./list-special-files.sh special-directory
special-directory/special-*
special-directory/special-fileB
special-directory/special-pluginA
```

Notice how the script works like a charm now? It can handle spaces in the directory name and also work with showing files which have special characters in their name, like `*`

Note, this is what I expected from my script. It's possible that you expected files with names containing `*` and wanted to expand the glob, due to some reason. I'll leave that to you to decide how you want to manage the errors from `shellcheck` in such cases. `shellcheck` wiki has information on how to ignore errors - https://github.com/koalaman/shellcheck/wiki/Ignore

Just one more example with the same code. Remember how I mentioned that it's cool that `shellcheck` mentions exactly what part of the code / variable to quote? If it hadn't done that, I could have quoted the code like this -

```bash
#!/bin/bash

dir=$1

for file in "$dir/special-*"; do
  echo "$file"
done
```

Notice the `"$dir/special-*"` and that's actually a blunder mistake 😅 But I'm no expert in bash script and might overlook that too, LOL! But some manual testing, or just running `shellcheck` can help find issues

```bash
$ ./list-special-files.sh "cool directory"
cool directory/special-*

$ ./list-special-files.sh special-directory
special-directory/special-*
```

Notice how it gives only one output? Exactly, there's a problem 😅

```bash
$ shellcheck list-special-files.sh

In list-special-files.sh line 5:
for file in "$dir/special-*"; do
            ^--------------^ SC2066: Since you double quoted this, it will not word split, and the loop will only run once.

For more information:
  https://www.shellcheck.net/wiki/SC2066 -- Since you double quoted this, it ...
```

`shellcheck` clearly mentions that I have double quoted the whole thing, so basically, there's only one element to loop through for the `for` loop and I'll only have one output, the exact string `"$dir/special-*"` after variable substitution 😅

Need to be careful to not use the double quotes in the wrong place(s) and also try to use `shellcheck` to check for errors! :)

These are just some examples by the way. Looks like `shellcheck` has many rules / checks to find issues / bugs in shell scripts. You can find all of these rules in the `shellcheck` wiki - https://github.com/koalaman/shellcheck/wiki in the `Pages`

## Shellcheck interfaces

There are many ways to use `shellcheck` ;)

You can use the online web user interface (UI) present in the website - https://www.shellcheck.net/ . ⚠️ Note that your shell script code is sent to a backend server and then checked. So, if you don't feel comfortable letting your code reach some third party servers, you can avoid using this, to prevent leakage of sensitive code

You can of course use the local offline `shellcheck` command line interface (CLI) tool like the above examples

You can also integrate this CLI tool in your Continuous Integration (CI) pipelines to run automated checks 😁 More about CI/CD here - https://github.com/koalaman/shellcheck/#in-your-build-or-test-suites

For development, instead of running the `shellcheck` command manually every time you make changes to the code or saving or committing the code to version control systems, I would recommend using plugins / extensions of `shellcheck` tool in your text editors or Integrated Development Environments (IDEs), which can give you feedback immediately and automatically when writing the shell script code :)

You can find more info about usage in editors here - https://github.com/koalaman/shellcheck#user-content-in-your-editor

The official wiki about usage is here - https://github.com/koalaman/shellcheck/#how-to-use

## Shellcheck support for different shells

Initially I didn't check too much on what kind of shell scripts `shellcheck` supports. But looking at one of the errors, I realized that it supports only a few shell scripts. Below is a sample `zsh` based shell script

```bash
$ vi shell-check-support.sh
$ cat shell-check-support.sh
#!/bin/zsh

echo "ok"
$ chmod +x shell-check-support.sh
$ ./shell-check-support.sh
ok
$ shellcheck shell-check-support.sh

In shell-check-support.sh line 1:
#!/bin/zsh
^-- SC1071: ShellCheck only supports sh/bash/dash/ksh scripts. Sorry!

For more information:
  https://www.shellcheck.net/wiki/SC1071 -- ShellCheck only supports sh/bash/...
```

You can read more about it here - https://github.com/koalaman/shellcheck/wiki/SC1071

In short, it supports only - bash, ksh, dash and POSIX sh, according to the wiki

I think it's a good enough support. I mean, as someone who writes scripts, I usually just use `bash` which has good enough features. Also, you can find `bash` in most environments, if not all. So, I'm okay with no support for `zsh`, `fish` and any other shells. I think they are probably more helpful for interactive usage, but for automation, maybe `bash`, `sh` is enough? But I don't know much about the other shells, so I guess I'll not comment more on it 🙈

`shellcheck` wiki also mentions how it doesn't support other scripting languages like python, php etc. I guess you can simply look for linters specific to those programming languages :)

## Conclusion

Now you know that you can use linters to lint your shell script code like any other programming language code! 😁

For any information relating to `shellcheck`, there are lot of online references including the `shellcheck` website, code (open source) and wiki. Below you can find some reference links that I noticed while reading about `shellcheck` :)

## References:

https://duckduckgo.com/?q=shellcheck 😛

https://www.shellcheck.net/

https://github.com/koalaman/shellcheck

https://github.com/koalaman/shellcheck/wiki/

https://github.com/koalaman/shellcheck#user-content-in-your-editor

https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck

https://plugins.jetbrains.com/plugin/10195-shellcheck

https://www.tecmint.com/shellcheck-shell-script-code-analyzer-for-linux/

https://hackage.haskell.org/package/ShellCheck

https://github.com/marketplace/actions/shellcheck
