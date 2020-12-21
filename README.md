# blog

This is the code for my blog! It's built using [hugo](https://gohugo.io) 😎

# Development

```
$ git clone git@github.com:karuppiah7890/blog
$ cd blog
$ git submodule init
$ git submodule update # to get the hugo theme
$ make dev # serve wiht draft mode on
$ make # to build site for prod
```

To get the latest ananke hugo theme

```bash
$ cd themes/ananke
$ git pull --rebase --verbose
```

If there are new commits, there will be changes in `themes/ananke` according to
this blog git repository like this

```bash
diff --git a/themes/ananke b/themes/ananke
index 2d6a91d..ce48112 160000
--- a/themes/ananke
+++ b/themes/ananke
@@ -1 +1 @@
-Subproject commit 2d6a91d48a283ef1fea71c149ee3c570627d2ae3
+Subproject commit ce48112de29f91330ec4da2bfb7ed14d0ee1a19f
```

