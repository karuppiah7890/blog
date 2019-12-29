---
title: "Helm values schema gen Plugin"
date: 2019-12-28T17:31:50+05:30
---

### tldr

```
$ helm plugin install https://github.com/karuppiah7890/helm-schema-gen
$ helm schem-gen <values-yaml-file>
```

GitHub repo for the plugin - https://github.com/karuppiah7890/helm-schema-gen

### Longer version

Today I finally created a [Helm](https://helm.sh) plugin that I wanted
to for so long. The repo is [here](https://github.com/karuppiah7890/helm-schema-gen)

In this post I'm going to tell the story behind the plugin. First the
what, then the why, and a bit of how.

#### The What

Recently, helm released their [version 3](https://github.com/helm/helm/releases/tag/v3.0.0)
which was a [big](https://www.cncf.io/announcement/2019/11/13/helm-reaches-version-3/)
[thing](https://www.youtube.com/watch?v=afCRt5Gd6Rk). It has some
really cool features and you can expect more in the v3 series

One of the features in v3 is, checking the schema of `values.yaml`
using [json schema](https://json-schema.org/). The schema is present
inside the helm chart as a file called `values.schema.json` which
contains the json schema. So, now if you make mistakes in the
`values.yaml` you will know it if you have the schema file with proper
schema definition. Schema validation check happens whenever you run
`lint`, `template`, `install` or `upgrade`. You can read more about it
[here](https://helm.sh/docs/topics/charts/#schema-files)

Now that you know about this feature, I can tell about the helm plugin
I created. As you can already see in the title, I created a plugin to
generate a values schema json for a given `values.yaml`.

#### The Why

The why might look very obvious. But I actually have multiple whys and
not all of them are obvious I think. The first reason for creating the
plugin is - ease of use. So, when I saw this feature in Helm v3, I was
quite excited to check it out. I wanted to try it out with the
default chart that comes with this command:

```
$ helm create test
```

But when I saw the big `values.yaml` in the `test` chart, I was
confused as to how I would write a very big schema file for it.
I was also thinking why the default chart did not have a schema
file too, so that I could try it out. I
[asked the maintainers about this](https://github.com/helm/helm/issues/6985)
and they felt that it's better to keep the advanced features out of the
default helm chart, so that it doesn't confuse the first time users.
So, I set out to try the feature by writing the schema file by hand
and by checking the docs on [json schema](https://json-schema.org/)
and the
[example in the helm docs](https://helm.sh/docs/topics/charts/#schema-files).
I didn't write the whole schema file for the given `values.yaml`. I had
just written the schema for some keys and checked how it works while
linting and templating.

And then suddenly I had this idea! Why do I have to write this big
thing by hand? It's so tedious and not easy. Also, I have the
`values.yaml`, why can't I detect the type of each and every key and
value and create the json schema? And that's how it all started.

Also, there are other reasons to create the plugin - many might still
be using helm v2, and it will take some time to move to helm v3. But
the migration to helm v3 should be really easy. That's why the
maintainers created [helm-2to3](https://github.com/helm/helm-2to3)
plugin for the easy migration. People migrate to new versions for the
features and support. If you don't know, Helm v2 support will stop at
[some point](https://helm.sh/blog/2019-10-22-helm-2150-released/#helm-2-support-plan).
And Helm v3 has really good features like values schema json, support
for pushing charts to OCI (https://www.opencontainers.org) registries,
removal of tiller and [some more!](https://github.com/helm/helm/releases/tag/v3.0.0).
Now, some of these features might need some work before you use the
feature. This the amount of work needed to adopt a feature a sofware
offers. In the case of the values schema json, this is the work where
you sit down and write the whole schema by hand, which will take a lot
of time if you maintain a lot of charts. Also, why do by hand when a
computer can do it for you? 😉 So yeah, that's another motivation and
the why for creating the plugin. Easy adoption of new features. It's
pretty important - because, marketing the new version of a software is
one thing, which leads to people seeing it. And when more people see
it, it means good marketing has been done. And then when people use
the new version of the software, then that's conversion, and that's
what matters. And that happens only when adoption of the new version
of the software is easy. Adoption includes being able to migrate to
the new version easily and also being able to use the new features
easily, which is usually the reason to even upgrade, apart from
support. So, this plugin will help people move to Helm v3 and be able
to use one new feature easily - the value schema json check feature.

#### The How

I'll tell a bit of the how I created the plugin.

When I got the idea, I immediately started checking out golang
libraries that can help me with finding the type of an unstructured
object (`values.yaml` data). I
found [one good library](https://github.com/mcuadros/go-jsonschema-generator),
but it worked for structured data like many others. I was
meddling with it to make it work for unstructured data. And then I left
it at some point. And then after a month, I recently started working on
it. I finally made the library work
[in my fork](https://github.com/karuppiah7890/go-jsonschema-generator).
And then created a separate repo for the command line tool for the helm
plugin. Now I spent hours checking how it's all done by checking code
of other helm plugins. And I had decided to use [urfave/cli](https://github.com/urfave/cli)
at first, but the UI for it seemed a bit different, compared to the
natural UI of [cobra](https://github.com/spf13/cobra) which I saw in
other plugins. It felt like I had to customize quite a bit in
`urfave/cli` library, so I just stopped and switched to `cobra`.
And then I chose to build the simple binary for the plugin. I ran it
a few times and it worked. And then I wanted to create a release
strategy, so that people can install the plugin and use it. I used
[`goreleaser`](https://goreleaser.com/) to release the binary, and
for CI/CD I used [GitHub Actions](https://github.com/features/actions).
I had to create a shell script for installing the plugin. I noticed
many other plugins using a script that they had written or copied from
another plugin. I didn't want to copy it or maintain it really, and I
knew [`godownloader`](https://github.com/goreleaser/godownloader) is
cool, so I just used it and wrote a two liner script on top of it for
customization. And then I released it finally! 

I tried out the plugin and everything worked really smooth! And then
I decided to write a small blog post about it, and it turned out to be
this big 👆! 😛

That's all! Have fun trying out the plugin!

```
$ helm plugin install https://github.com/karuppiah7890/helm-schema-gen
$ helm schem-gen <values-yaml-file>
```

