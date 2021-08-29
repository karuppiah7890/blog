---
title: "Building CLI tools"
date: 2021-08-29T12:40:30+05:30
draft: true
---

Progress

Positioning of arguments? Or use flags

Refer to Heroku CLI guide? Other CLI guides?

Usage in CI vs usage in local in dev machines - interactive usage - progress bars with rendering progress in the same line of output, but it's not possible in CI

Showing actionable error messages

Single standalone binary not dependent on anything else or dependent on very minute things - awesome! Ideally I would choose Golang, or something like Rust to build the tool. Could use other languages too, to build standalone tools. Opposite? Need a runtime installed like Python, JVM, NodeJs, Ruby etc, need shared libraries like `.so` files in Linux, where CLI tool depends on the libraries at runtime - more like "dynamically linked library" compared to the other side of "statically linked binary"

Above makes it easy to install and use. We need to also distribute software well - for easy install and upgrade, to all supported and tested platforms. Use package managers, shell scripts for easy install. One line install is awesome

Test UX during development - installation, uninstallation, upgrade, among others

CLI tool autocompletion for usage - provide it through CLI command itself, like `helm completion`, `ko completion` for auto complete in different shells like bash, zsh, fish etc based on whatever is supported

Ship configuration also as part of the CLI tool. Let there be some `init` command to initialize the config in the system. For example `helm init` does initialization

The single standalone binary should be able to take care of everything as much as possible

Provide help for every command, sub command and flag and also show examples. Let the user read everything from the shipped code rather than getting drowned in docs, though docs is also an option. Try to help users during the interactions itself, instead of redirecting them to docs always. Use the CLI output to communicate things as much as possible instead of depending on docs, forums etc. When showing error, also give ideas on fix if it's surely known. Use human readable error messages
