---
title: "Linting Shell Scripts using shellcheck tool"
date: 2021-05-18T23:35:20+05:30
draft: true
---


Code examples:

### Good code but can have problems

`list-special-files.sh`

```bash
#!/bin/bash

dir=$1

for file in $dir/special-*; do
  echo $file
done
```

```bash
$ vi list-special-files.sh
$ chmod +x list-special-files.sh
$ mkdir "cool directory"
$ cd "cool directory"
$ touch meh
$ touch special-pluginA
$ touch special-fileB
$ cd ..
$ ./list-special-files.sh "cool directory"/
cool
directory//special-*
```


### Better code

`list-special-files.sh`

```bash
#!/bin/bash

dir=$1

for file in "$dir"/special-*; do
  echo $file
done
```

```bash
$ ./list-special-files.sh "cool directory"/
cool directory//special-fileB
cool directory//special-pluginA
```

Need to be careful to not use

```bash
for file in "$dir/special-*"; do
  echo $file
done
```

As that leads to - 

```bash
$ ./list-special-files.sh "cool directory"/
cool directory//special-*
```


References:

https://duckduckgo.com/?t=ffab&q=shellcheck&ia=images

https://www.shellcheck.net/

https://github.com/koalaman/shellcheck

https://marketplace.visualstudio.com/items?itemName=timonwong.shellcheck

https://www.tecmint.com/shellcheck-shell-script-code-analyzer-for-linux/

https://plugins.jetbrains.com/plugin/10195-shellcheck

https://hackage.haskell.org/package/ShellCheck

https://github.com/marketplace/actions/shellcheck
