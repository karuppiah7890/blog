---
title: "Step CLI to decode JWT tokens"
date: 2020-09-17T21:05:08+05:30
---

# tldr;

`step` GitHub repo - https://github.com/smallstep/cli

```bash
$ # Reading from keyboard / standard input after running command.
$ # Run command and then paste the JWT token as input and press Enter.
$ step crypto jwt inspect --insecure
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}

$ # Reading from file
$ cat jwt-token.txt | step crypto jwt inspect --insecure
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}

$ # Reading from environment variables
$ echo $JWT_TOKEN | step crypto jwt inspect --insecure
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```


Recently I was trying to decode the information present in a JWT token to
understand what's the information in it.

Initially I forgot the concept of JWT and thought I would need the secret key
it was signed with to even read the info in it. I realized that wasn't the case
when I recalled it by checking out jwt.io site's live debugger for decoding
jwt or encoding to jwt. The secret key is needed only to sign or verify. And
I knew that in the case of PKI (Public Key Infrastructure), just the public key
is enough to verify the JWT token but to sign, you would need the private key,
yes.

Now for the decoding - I had a JWT token that I got from a Java Service. Now
I could use jwt.io to find the information in it. The thing is this - I
usually don't trust any other external third party sources when it comes to
keying in sensitive and confidential information, especially related to my
company client project. I did later check about jwt.io - I noticed that they
do it all in the browser with Js - at least that's how it looks like, since I
didn't notice any network calls in the Network tab of my developer tools when
making changes in the live data in the debugger

Anyways, what I usually prefer is local tools - offline tools - of course, even if you
are online, you should be able to trust this software - something open source
and if you are very very suspicious about other's tools, then maybe even get the
source code and compile it yourself, and also check the source code line by line (:P),
instead of just getting the binary / executable and running it.

Now, back to the problem - I wanted offline tools and I think the easiest and
simplest to use is CLI tools. Let's face it - it's hard to build even CLI
interfaces, let alone build GUI interfaces which are fancy. GUI apps can also be
heavy given so many people just choose Electron or other heavy GUI frameworks
or development tools to build GUI apps. So I went ahead and searched GitHub
for "jwt cli"

https://github.com/search?utf8=%E2%9C%93&q=jwt%20cli

And found the first repo as - https://github.com/smallstep/cli

There is one more that I didn't open or use - https://github.com/mike-engel/jwt-cli .
I didn't even check for any other thing. The other results didn't even make much
sense.

Now, this is the first time that I noticed `step` tool. Previously when I had this
problem and my team mate was trying to write a command line utility to help with
some automation, he wrote a script - bash script and in that, a part of it was
providing JWT token and usually he was manually creating it using jwt.io and
I asked him not to do it due to the same reasons as before - security concerns.
So, as part of automating it and not using jwt.io, we were planning to use some
tool - but we felt that then everyone has to install some external tool, then
we found on the Internet some bash script to help with JWT encoding - but of course,
not everything was plain bash - there were tools - guess what tools? Of course
the standard crypto tool - `openssl`, and with it, JWT token encoding was done and
I think `jq` was used too, I'm unable to recall now. But yeah, that's how we had done
it, with bash and external tools.

Compared to that, `step` tool was just cool when I saw it. I could easily
install it with `brew`

```bash
$ brew install step
```

And I was easily able to decode my JWT token with just this -

```bash
$ # Run command and then paste the JWT token as input and press Enter.
$ step crypto jwt inspect --insecure
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```

To use it with files and environment variables, just do some bash tricks!

```bash
$ cat jwt-token.txt | step crypto jwt inspect --insecure
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}

$ echo $JWT_TOKEN | step crypto jwt inspect --insecure
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  },
  "signature": "SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```

The `--insecure` is because we are trying to avoid verification of the token.
https://smallstep.com/docs/cli/crypto/jwt/inspect/

I think it's a really cool tool! :)

The above example is from the jwt.io website. You can try with your own JWT
token.

`step` seems to have a lot of other features too!
https://smallstep.com/docs/cli/

It's by a company called smallstep https://smallstep.com/
