---
title: "Jest and TypeScript"
date: 2021-05-06T20:47:35+05:30
---

I was recently working on a front end project where the code was written in TypeScript. It was a React project, and it was bootstrapped with Create React App (CRA) tool. And the testing framework that we used was Jest.

The team quickly realized that the test code we wrote was not being type checked and we could see TypeScript type errors in our text editors and Integrated Development Environments (IDEs) but there were no such type errors when running the tests using `react-scripts` tool to run the tests which in turn used Jest

## Why type check test code too?

It does not matter what kind of project you are working on, front end or backend or anything. If the source code and test code is written in TypeScript, I believe that all this code needs to be type checked.

So, if your test code is not type checked, then that's a problem according to me.

Just think about it, you use TypeScript to get type safety and to catch a lot of projects during compile time. Now, if you are using TypeScript but are not getting those type safety and type checking benefits then why write code in TypeScript? That's exactly what's happening when your test code is not type checked.

> Note: Many have some controversial opinions on type checking in JavaScript world, but I'm not going to go there. I'm just going to stick to TypeScript and talk about the benefits that I see from my perspective.

If your source code is not type checked...that's a bigger problem. Ideally you would have to compile your source code to get JavaScript code to get the final usable code, so ideally source code type checking should happen as part of the build stage where compilation happens.

According to me, learning and writing code in TypeScript requires a bit more learning, especially when using TypeScript features, or else it's just plain JavaScript. If you are going far to learn and type in TypeScript, you might as well benefit from it, and get the core features like type checking or else it's just unnecessary complexity to have TypeScript if you don't type check your test code.

Also, when you do type checking of your test code, you might find out a lot of issues!

For example, below are the kind of issues that we found out in our project's test code

- Passing the wrong number of parameters in test code

    We passed wrong number of properties to our React components in our test code. This is not possible to do in source code as that has type checking. Since there was no type checking in our test code, we never found out this issue. But we could see IDE errors showing red squiggly lines to indicate such errors

    In some cases we intentionally passed lesser parameters to test something, but in reality, one cannot pass lesser parameters, so it was more like an invalid test.

- Passing the wrong type of parameters

    We passed the wrong type of properties to our React components in our test code. Sometimes it was just unintentional wrong type, sometimes it was intentionally wrong type to test some behavior. For example, when sending null to a non-null property, we were testing how the component works, but in reality, null can never be passed to the component

- Passing non-existent parameter values

    We were passing some non-existent property values to our React components which were present before but removed later in the source code due to change in implementation. So, it was more like dead code / dead properties in the test code.

There are many other kinds of issues you can find when you type check your test code written in TypeScript! You will write better tests when you type check your TypeScript test code too!

## Analysis and solutions for type checking test code

On analyzing why type checking was not happening for our Jest test code, I later found out that Jest by default supports TypeScript using Babel and is purely just transpilation, it doesn't type check the test code when the tests are run. Check this link here - https://jestjs.io/docs/getting-started#using-typescript to see this information

They recommend using the [`ts-jest`](https://github.com/kulshekhar/ts-jest) tool or the `tsc` TypeScript compiler or some other way to do type checking for the test code.

In our project I tried so hard to make [`ts-jest`](https://github.com/kulshekhar/ts-jest) work but somehow it didn't work. I have worked with [`ts-jest`](https://github.com/kulshekhar/ts-jest) in this project though - https://gitlab.com/snapping-shrimp/cocreate-remote-vscode where you can find the jest config file here - https://gitlab.com/snapping-shrimp/cocreate-remote-vscode/-/blob/main/jest.config.js

Since it was becoming hard to use `ts-jest` in our front end project, I settled for using `tsc` which didn't seem like a very bad idea. I mean, I just had to run the compiler and it just worked. The only thing to note is, the type checking for source code happened twice in our project. How you ask?

`tsc` - TypeScript compiler checks source code and test code

`react-scripts build` - TypeScript compiler checks the source code

`react-scripts test` - It did not type check test code

As you can see, our source code was type checked twice. But I figured that it's okay as long as we get the benefit of type checking the test code. I didn't look further into configuring the TypeScript compiler to look at only test code etc. Something to note is - the TypeScript compiler config we had was used for the building of the source code too, so I don't think I could have changed that, so I would have had to use a separate config to just type check the test code and ignore the source code.

Anyways, that was how I simply ran `tsc` and nothing else, and got type checking features for our test code too. I also added this command as part of our CI/CD pipeline stage which was run by Jenkins in our project.
