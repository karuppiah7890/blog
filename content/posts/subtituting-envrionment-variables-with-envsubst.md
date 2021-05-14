---
title: "Substituting environment variables with envsubst"
date: 2021-05-14T22:33:15+05:30
---

## TLDR;

```bash
$ man envsubst
```

Online man page - https://www.man7.org/linux/man-pages/man1/envsubst.1.html

```bash
$ cat sample.template.yaml 
shell: ${SHELL} # notice how we can use ${} format too
histsize: $HISTSIZE
home: ${HOME}
user: ${USER}

$ cat sample.template.yaml | envsubst '$SHELL ${HISTSIZE}'  > sample.yaml

$ cat sample.yaml 
shell: /bin/bash # notice how we can use ${} format too
histsize: 50000
home: ${HOME}
user: ${USER}
```

## Longer version

Me and my team mate are working on writing some automation scripts to automatically run some commands which can bootstrap a kubernetes cluster. One of the requirements we had was - injecting secrets in a YAML file

It's never a good practice to store secrets in plain text in a file and then commit it in a Version Control System. I wrote a blog about it recently - https://karuppiah7890.github.io/blog/posts/bytes-4-dont-use-plain-text-files-to-store-secrets/

For this reason, my team mate was following one of the best practices - which is to use environment variables in different places of the code - wherever the secrets are required. And then injecting secrets at runtime using the same environment variables

In our case, we were dealing with Amazon Web Services (AWS) client ID and secret key which are secrets and sensitive. And we wanted to put the secrets in a YAML file

To solve this, my team mate created a YAML template file and put placeholders for the secrets - using environment variable names. Now the only thing left out was - how to inject / replace the placeholders with actual values from the environment variables at runtime

There are probably many tools to help with this problem - to interpolate YAML template files with references to environment variables. Lot of templating tools out there

I had requested my team mate to checkout `envsubst` since I had used it before and felt it was pretty handy and cool. I think he tried out different ways and later mentioned `envsubst` was what he finally chose

Let's look at what is `envsubst` and how it helps in our use case

`envsubst` is a tool to substitute environment variables with their values. Let's look at a very basic example

```bash
# Note the single quotes. Don't use double quotes or the shell will substitute
# the environment variable before envsubst tool can
$ echo '${SHELL}'
${SHELL}

$ echo '${SHELL}' | envsubst 
/bin/bash
```

Notice how `envsubst` reads from the standard input and substitutes the environment variable `${SHELL}` with it's value `/bin/bash` which is my shell

This was just a basic example. `envsubst` works by using the standard input and standard output in general

For us to replace environment variables in a file, below are some ways to do it

```bash
$ vi sample.yaml

$ cat sample.yaml
shell: $SHELL

$ cat sample.yaml | envsubst 
shell: /bin/bash

$ cat sample.yaml 
shell: $SHELL

# another way
$ envsubst < sample.yaml 
shell: /bin/bash

$ cat sample.yaml 
shell: $SHELL
```

So, now we have seen how the input is given with files. These are generic examples of how shell users generally give file input in the form of standard input to programs.

Notice how the sample.yaml hasn't changed though? Now, we need the output back in the same file. Since the output goes to standard output, let's use some standard shell features to store it back in the file

```bash
$ cat sample.yaml 
shell: $SHELL

$ envsubst < sample.yaml > sample.yaml.output

$ cat sample.yaml 
shell: $SHELL

$ cat sample.yaml.output 
shell: /bin/bash

$ cat sample.yaml.output > sample.yaml

$ cat sample.yaml
shell: /bin/bash
```

You can use other tools and simple shell features to store the output in a file - same or different

`envsubst` also has some more features. I think two more. I'm gonna talk about one of them. So, apart from substituting environment variables, you can also mention exactly which environment variables need to be substituted, and others will be left out without substitution. See below for an example

```bash
$ cat sample.template.yaml 
shell: ${SHELL} # notice how we can use ${} format too
histsize: $HISTSIZE
home: ${HOME}
user: ${USER}

$ cat sample.template.yaml | envsubst '$SHELL ${HISTSIZE}'  > sample.yaml

$ cat sample.yaml 
shell: /bin/bash # notice how we can use ${} format too
histsize: 50000
home: ${HOME}
user: ${USER}
```

Notice how only `SHELL` and `HISTSIZE` environment variables got substituted? Also note the usage of `${HISTSIZE}` and `$HISTSIZE`. The `${}` and `$`, both formats can be used to mention the environment variable names in the inputs - in standard input and in the `envsubst` command line arguments

You want to read more on `envsubst`? You can checkout the help of `envsubst` to get some more details -

```bash
$ envsubst --help
Usage: envsubst [OPTION] [SHELL-FORMAT]

Substitutes the values of environment variables.

Operation mode:
  -v, --variables             output the variables occurring in SHELL-FORMAT

Informative output:
  -h, --help                  display this help and exit
  -V, --version               output version information and exit

In normal operation mode, standard input is copied to standard output,
with references to environment variables of the form $VARIABLE or ${VARIABLE}
being replaced with the corresponding values.  If a SHELL-FORMAT is given,
only those environment variables that are referenced in SHELL-FORMAT are
substituted; otherwise all environment variables references occurring in
standard input are substituted.

When --variables is used, standard input is ignored, and the output consists
of the environment variables that are referenced in SHELL-FORMAT, one per line.

Report bugs in the bug tracker at <https://savannah.gnu.org/projects/gettext>
or by email to <bug-gettext@gnu.org>.
```

There's also a man(ual) page for it

```bash
$ man envsubst
```

Online man page - https://www.man7.org/linux/man-pages/man1/envsubst.1.html

If you use [`tldr`](https://tldr.sh/) tool, that can also help

```bash
$ tldr envsubst

envsubst

Substitutes environment variables with their value in shell format strings.
Variables to be replaced should be in either `${var}` or `$var` format.
More information: <https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html>.

- Replace environment variables in stdin and output to stdout:
    echo '$HOME' | envsubst

- Replace environment variables in an input file and output to stdout:
    envsubst < path/to/input_file

- Replace environment variables in an input file and output to a file:
    envsubst < path/to/input_file > path/to/output_file

- Replace environment variables in an input file from a space-separated list:
    envsubst '$USER $SHELL $HOME' < path/to/input_file
```
