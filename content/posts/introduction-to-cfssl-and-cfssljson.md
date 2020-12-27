---
title: "Introduction to cfssl and cfssljon"
date: 2020-12-27T14:42:35+05:30
draft: true
---

https://cfssl.org/

https://blog.cloudflare.com/introducing-cfssl/

https://github.com/cloudflare/cfssl

Where does it help?
How does it compare to openssl? Is it just helping you to convert a big openssl
command into a small cfssl and cfssljson command with all the input / details
in the config /csr json files? Does it just reduce the number of openssl
commands maybe? Hmm.
Or does it also help with something more? For example bundling multiple CA
certificates

What's the other binaries in the releases?
https://github.com/cloudflare/cfssl/releases
https://github.com/cloudflare/cfssl/releases/tag/v1.5.0

For example cfssl bundle , cfssl cert info (certinfo is infact a sub command in
cfssl), cfssl new key, cfssl scan, mkbundle, multirooca

How does cfssl help with creating certificates for supporting multiple browsers?

cfssl as a web service? Hmm

How did I know about cfssl? I noticed it when trying out kubernetes hard way
tutorial and other similar kubernetes cluster bootstrap tutorials that used
cfssl in their scripts or instructions
