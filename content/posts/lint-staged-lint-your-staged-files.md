---
title: "lint-staged: Lint Your Staged Files"
date: 2020-11-16T09:11:20+05:30
---

In my project, we use this tool called [lint-staged](https://github.com/okonet/lint-staged)
which is pretty awesome! I wanted to write this blog post to talk a little bit
about it and also mention some of the cool things I liked about it! :D

To start off, I work on a React project with Typescript. We need to ensure we
follow some good practices as much as possible, so we use ESLint in our project
We also use Prettier for formatting, so that our code looks neat and the same
always no matter who worked on it, letting us read code in a consistent manner
and not fight about formatting rules. If you have not heard about linters and
formatters, I would recommend checking it out first before reading this post :)

To run our linters and formatters, we could go two ways - either run them always
on the whole project or just run for the changes we make for each commit.
Running linters for the whole project is going to take a lot of time since it
has to run for all files and that's going to be slow and going to be a bad
Developer Experience (DX, like UX). In this aspect, the other approach sounds
good - just run linters and formatters for the changes we make for each commit.
I mean, this rule can be applied for almost anything - building / compiling too,
to compile only files that have been changed and the files related to those
changes too maybe but that's it, nothing unrelated.

What are the problems with the second approach though? One could say that "Hey,
if I just run linting and formatting for the changes I make, that sounds nice,
because I don't have to see some weird issues that is caused by some file that
I didn't edit. But then, the whole point of linting is lost right? I mean, then
I just worry about my code changes and not worry about the whole project, which
might be breaking some lint rules or formatting rules"

I think this is a valid concern. I would say that - it's not exactly your worry
to start with - as in, first you need to worry about your code changes. Once
that's resolved, for the whole project, ideally people making the other changes
need to take care of it. And to ensure this, you can run the "lint check" as a
job in your Continuous Integration (CI) Pipeline, so that when someone pushes
code that is not properly formatted or has lint issues, the "lint check" job
will run the linting and formatting check in the whole project and will error
out for any errors, resulting in a red CI pipeline. Then the people who broke
the pipeline can check it out and fix it. Or, you could fix it too, but in a
separate commit probably. :)

Now that we have agreed that we can run linting and formatting on just the
files changed - that is, the files in each commit, how does one do it? Usually
people use git hooks to do such kind of code quality tests - to run linting,
formatting, tests and similar checks. For example, you could run a git pre
commit hook, which runs before each commit and does all the code quality checks.
Either you could do all the scripting for git hooks or you could use existing
tools and maybe not reinvent the wheel and spend your time on something else :)

This is where [lint-staged](https://github.com/okonet/lint-staged) tool comes
into the picture. Usually one would run the code quality check on the whole
project in the git hook scripts. `lint-staged` helps you to run any process or
jobs on just the files changed and staged for the commit. If you have 10
modified files in your git repository, and you have staged only two files for
committing, `lint-staged` will work on only those two files and nothing else.

Let's see how to setup `lint-staged` and then see it in action! :)

As an example, I'm going to take this project -
https://gitlab.com/snapping-shrimp/cocreate-remote-vscode/

It has ESLint and Prettier, and also `lint-staged`. I will take you through what
has been done for the project to setup `lint-staged`. Something to note is,
`lint-staged` has some automated way of doing setup in
[their readme](https://github.com/okonet/lint-staged#installation-and-setup).
I would recommend checking it out and trying it out. Below I'm going to explain
how to do it manually as that's how I did it 😅 You can skip the below section
to and move the [Seeing lint-staged in action](#Seeing-lint-staged-in-action) section

## Setup

Assuming you have already setup code quality checks like test framework,
linters, formatters, I'm going to move forward to the next step of setting up
`lint-staged`.

For `lint-staged` to work, you need NodeJS. This is because `lint-staged` is
written in JavaScript. If you work on a JavaScript / TypeScript project, it
should be pretty straight forward to use `lint-staged`. Or else, I would
recommend checking out other tools in the ecosystem of your language, or you
could just use NodeJs just for some tools like this. It's totally your call.

Now that we have sorted out the language/environment/ecosystem, let's move to
the installation of `lint-staged`. `lint-staged` given that it works with staged
files and for each commit, usually people use it with another tool called
[`husky`](https://github.com/typicode/husky) which helps with git hooks,
including pre commit hook. But it's not mandatory though, you could still use
`lint-staged` without `husky`. Now, moving on to the installation.

```bash
$ npm install --save-dev lint-staged
```

If you are going to use [`husky`](https://github.com/typicode/husky), check out
which version you can use - v4 or v5 and then set it up.

After the `lint-staged` installation is done, all you gotta do is, go to your
`package.json` and put this in

```json
{
  "lint-staged": {
    "*.js": [
      "linter-command",
      "formatter-command",
      "test-command",
      "any-command"
    ]
  }
}
```

If you are not comfortable using your `package.json` as a place for
configuration for tools, there are other ways to configure too, checkout the
tool's [readme](https://github.com/okonet/lint-staged#configuration)

The above configuration means that "hey lint-staged, when you run, check the
staged files, and out of those files, find any staged files with the `.js`
extension and run the following list of commands for those files"

Something to note is, each command needs to have certain properties and the
`lint-staged` configuration also must be according to that. For example, if
your lint command is like this

```bash
$ linter-command index.js
```

Then you just use `linter-command` in your `lint-staged` configuration.
`lint-staged` will take care of passing the file path of the staged files as
it's the one that knows which files are staged and it's a dynamic thing but your
configuration is just static and not dependent on that. But your
`linter-command` should run only for the given file path. Ensure that's the
case. Or else, let's say your `linter-command` works on the whole project no
matter what, then there is no use of using a tool like `lint-staged` whose sole
purpose is to run commands on only the files that are staged and are going to be
committed in git.

So, never pass file path related information in the `lint-staged` configuration.
Once that's done, all you gotta do is, run `npx lint-staged` in your git pre
commit hook or any hook that you want to use. If you are using husky v4, then
it would look like the below in your `package.json`

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
}
```

Follow the similar thing in case you are using some other tool or your own
script for git hooks, or husky v5. Just run the command `lint-staged` :)

## Seeing lint-staged in action

I'm trying out `lint-staged` in this project -

https://gitlab.com/snapping-shrimp/cocreate-remote-vscode

I'm making a change in a TypeScript file, where I change the word `Signed in` as
`Logged in`. Look at what happens in my shell when I run the git commands

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/extension.ts

no changes added to commit (use "git add" and/or "git commit -a")

$ git diff
diff --git a/src/extension.ts b/src/extension.ts
index 273d95f..8c2041a 100644
--- a/src/extension.ts
+++ b/src/extension.ts
@@ -11,9 +11,8 @@ export function activate(context: vscode.ExtensionContext): void {
           createIfNone: true,
         })
         .then(
-          (authSession) => {
-            vscode.window.showInformationMessage(
-              `Signed in as: ${authSession.account.label} (github)`
+          (authSession) => { vscode.window.showInformationMessage(
+              `Logged in as     : ${authSession.account.label} (github)`
             );
           },
           (err) => {

$ git add .

$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   src/extension.ts

$ git commit -m "dummy change"
✔ Preparing...
❯ Running tasks...
  ❯ Running tasks for *.{js,ts}
    ✔ eslint --fix
    ⠙ prettier --write
  ↓ No staged files match *.{json,yml,md} [SKIPPED]
◼ Applying modifications...
◼ Cleaning up...
```

You can see how there are some set of tasks that are running when I run git
commit command. This is because of the git pre commit hook that I have setup
with husky v5 :)

Finally it looks like this

```bash
$ git commit -m "dummy change"
✔ Preparing...
✔ Running tasks...
✔ Applying modifications...
✔ Cleaning up...
[main 41afab4] dummy change
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git show
$ git show
commit 41afab43431cb2be65edac395577bf52fa13f3e4 (HEAD -> main)
Author: Karuppiah Natarajan <karuppiah7890@gmail.com>
Date:   Mon Nov 16 17:01:30 2020 +0530

    dummy change

diff --git a/src/extension.ts b/src/extension.ts
index 273d95f..2fc3d5c 100644
--- a/src/extension.ts
+++ b/src/extension.ts
@@ -13,7 +13,7 @@ export function activate(context: vscode.ExtensionContext): void {
         .then(
           (authSession) => {
             vscode.window.showInformationMessage(
-              `Signed in as: ${authSession.account.label} (github)`
+              `Logged in as     : ${authSession.account.label} (github)`
             );
           },
           (err) => {
```

You can see how the git commti changes are different from what we saw before.
Prettier has formatted the code while doing the commit and it has also added
the formatted code change and then did the commit.

So, `lint-staged` can not just run commands on your staged files, if your
commands are not just checks and if they also automatically change code, then
`lint-staged` can add / stage those changes alone if related code was already
staged, and then it will commit it all together ;)

I have also tried to partially add / stage files and try to use `lint-staged`
and it has worked well that time too :D Also, if you make changes that are
reverted because of the commands that run during `lint-staged`'s processing,
then it can lead to an error like the below

```bash
$ git commit -m "dummy change"
✔ Preparing...
✔ Hiding unstaged changes to partially staged files...
✔ Running tasks...
✖ Prevented an empty git commit!
↓
  ✖ lint-staged failed due to a git error. [SKIPPED]
✔ Reverting to original state because of errors...
✔ Cleaning up...

  ⚠ lint-staged prevented an empty git commit.
  Use the --allow-empty option to continue, or check your task configuration

husky - pre-commit hook exited with code 1 (error)
```

As you can see, `lint-staged` errored out saying it was an empty git commit - a
commit with no changes. No changes because all the changes I made were
formatting changes, which were wrong according to prettier and it fixed it back
and on doing so, there was no changes left to even be committed to git.

## Conclusion

I would recommend you to try out `lint-staged` and give it a go in any of your
projects. You can also look for similar tools in case you want some tool native
to your eco system in case your's is not a JavaScript project. `lint-staged`
would work too ;) :)

If you have any questions regarding `lint-staged`, let me know in the comments
below and I'll try to answer them :)
