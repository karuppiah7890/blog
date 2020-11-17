---
title: "Digital Signatures"
date: 2020-11-16T20:21:57+05:30
---

If you don't know what a digital signature is, in layman terms you may think of
this, a photo of your physical signature

![digital-signature-1](/blog/img/digital-signatures/digital-signature-1.jpg "digital-signature-1")

###### Photo by [Lewis Keegan - Skillscouter.com](https://unsplash.com/@skillscouter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/signature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

May be you could say that's true, but only if it's layman slang. Officially, in computer science, the concept of digital signature has a different meaning.

It does denote someone's or some entity's signature, in a digital world. Yes. So, that's not wrong. You may ask then what's wrong with above signature. Well the above signature has issues. Let's start seeing what and why.

Usually, in the real world with physical signatures, signatures are used by someone to tell that they agreed to a formal document with a sound mind, right? And you expect some features from signatures. For example you don't want someone to be able to forge your signature. When I was young, it's funny how I used to try to keep my signature cool and awesome and tough to forge. When I look at it now, it's actually pretty simple. Probably even a petty thief can copy me with a few minutes of practice. I don't know how big shots make sure that their signature is not forged. Anyways. Let's take the above digital signature. How easy is it to forge it in the digital world? I mean, it's an image of a signature. It's a digital thing. And one property about digital things is - you can make how much ever copies you want, right? So, in the above case, if someone just copies and pastes the image in tons of documents, it would mean that I agreed to all those documents, right? As simple as that. It won't even take a few minutes to do that if that's automated. So, how does one put their signature in a digital document, with it not being forged by someone else?

Let's try to understand the kind of features we need for our digital signature and what kind of problems we need to solve.

The goal is - I need to be able to sign a digital document and be able to send the digital document to anyone. Anyone should be able to get the document and be able to see the signature and also be able to verify that I was the one who signed it and that it's very hard or impossible for someone to forge it. Given that others have access to my signature as I sent them the digital document, they should not be able to copy and paste my signature on to other digital documents that I never intended to sign and hence forging my signature. But they can share the exact same document, for others to see that I have digitally signed it.

So, how can we achieve this?

This is where cryptography comes into play!

Let me give one example of what you can do for a digital signature. If you are
not familiar with asymmetric / public key cryptography, I would recommend
checking it out first.

So, what you can do is - since your private key is private to you and you alone,
and you can share your public key to everyone, you take your digital document,
you create a copy out of it, and then you encrpyt the copy with your
private key. This private key encrypted document is your digital signature. And
then you put both the document and the digital signature together and then send
it to anyone who needs to see it. How does that sound? So, how does one verify
that it's your signature? They can take the digital signature and decrypt it
with your private key, which will give the digital document. They can verify the
actual document and the document they got from the decryption. If both are same
then it means that it's a valid signature, meaning - it was signed by you or
else the decryption with your public key wouldn't work, it works so it means it
was encrypted with your private key and ideally only you have access to your
private key, or else it would mean your private key has been compromised which
is a big issue. And what else does it mean? It also means that the signature was
put only for that particular document and not for any other document, this is
ensured by the fact that the digital signature is just a private key encrypted
form of the exact same document. No one can forge your digital signature,
because they don't have your private key, but they can still share the exact
same document and the signature with others so that they can also see it and
also verify it too if needed. They just need access to your public key, which
is usually available publicly in a key server. Ensure that you have a secure
distribution of your public key and also have secured your private key. This
will only work properly based on the security of your keys.

##### Signing when using full document for signature:

![digital-signature-signing-with-full-doc](/blog/img/digital-signatures/digital-signature-signing-with-full-doc.svg "digital-signature-signing-with-full-doc")

##### Verifying when using full document for signature:

![digital-signature-verification-with-full-doc](/blog/img/digital-signatures/digital-signature-verification-with-full-doc.svg "digital-signature-verification-with-full-doc")

Now, are there any problems with the above solution? 🤔 There's one small thing.
It works, yes. But, if you think about it, digital signatures in the real world
are needed for a lot of things - executables / software, tools, emails and what
not. Let's take the example of software. Let's say I have a software that's 2GB
in size. If I need to put my signature on it and send it over to someone, then
based on the above mechanism, I need to send the 2GB file and another ~2GB file
which is the private key encrypted form of the actual digital file. So, we now
have ~4GB of data that needs to be transferred. Now, isn't that a lot? Every
time I want a digital signature, I double the amount of data I need to sent to
the other person. Now, that's a pain, right?

There's a soluton to that too ;) So, let's get to the core of why we use the
exact document and use that for our encryption with private key and consider it
as our document's digital signature? Well, it's because, we want to show that
the signature is ONLY for that digital document and not for anything else - any
other digital document out there. So, the need is to prove that "I signed this
exact same document and not something else. So, you can't forge it for signing
another document". To identify digital assets, or to verify two digital assets
are the same, you could either check every byte of data in the digital asset, or
you can choose to use the digital fingerprint of the data to check if they both
are exactly equal. Hashing is the digital fingerprint I'm talking about. There
are lot of cryptographic hashing functions. For example SHA-2, SHA-3, MD5. So,
what happens is - for your digital signature, you hash the document using a
hashing function and then your encrypt the hashed value with your private key
and there's your digital signature. Given hashes are very small, it will not add
too much to the size of the file you need to transfer. On the other side, for
the verification of the digital signature, they have to take the document and
hash it using the same hashing function you used, and then take the digital
signature and decrypt it with your public key to get a hashed value. Now both
the hash values are checked. If they are equal, the digital signature is said to
be valid and verified.

##### Signing when using hash for signature:

![digital-signature-signing-with-hash](/blog/img/digital-signatures/digital-signature-signing-with-hash.svg "digital-signature-signing-with-hash")

##### Verifying when using hash for signature:

![digital-signature-verification-with-hash](/blog/img/digital-signatures/digital-signature-verification-with-hash.svg "digital-signature-verification-with-hash")

So that's how you put digital signatures and verify it too, using hashes. And
each signature is specific to each digital document / digital file.

Apart from size issues / concerns, there are other reasons to use hash instead
of the whole document for the digital signature. You can check it in this
[section of wikipedia](https://en.wikipedia.org/wiki/Digital_signature#Method).
Here's an excerpt from the wikipedia page regarding all the reasons to use
hashes

```
There are several reasons to sign such a hash (or message digest) instead of
the whole document.

For efficiency:
  The signature will be much shorter and thus save time since hashing is
  generally much faster than signing in practice.

For compatibility:
  Messages are typically bit strings, but some signature schemes operate on
  other domains (such as, in the case of RSA, numbers modulo a composite
  number N). A hash function can be used to convert an arbitrary input into
  the proper format.

For integrity:
  Without the hash function, the text "to be signed" may have to be split
  (separated) in blocks small enough for the signature scheme to act on them
  directly. However, the receiver of the signed blocks is not able to
  recognize if all the blocks are present and in the appropriate order.
```

## Other methods for digital signatures

There are other ways to do digital signatures too. For example, instead of using
asymmetric encryption, one could use symmetric encryption, where the sender and
the receiver, both have the same shared key. To sign and verify, you use the
same key. But remember, this would mean that they can also sign some digital
document with that shared key and could also share that shared key with
someone else. I have not tried this kind of digital signature before, but it
exists and it's possible.

## Usage of digital signatures in the real world

Where are digital signatures used? In a lot of places. For example - SSL
Certificates for HTTPS websites, JWT tokens, Emails, Signing git commits. These
are just the ones that I know of. There could be more.

If you want to try out signing a document, I would recommend trying out a
command line utility that I know of, it's called `gpg` which is short for
Gnu PG - Gnu Privacy Guard -

https://www.gnupg.org/gph/en/manual/x135.html

Below is an example of trying out `gpg`. I try to create a detached digital
signature based on the concept we just spoke about in the above sections

```bash
$ # Create a digital signature
$ gpg --output README.md.sign --detach-sign README.md
$ # Check the digital signature that has been created
$ ls README.md*
README.md       README.md.sign
$ # Verify the digital signature
$ gpg --verify README.md.sign README.md
gpg: Signature made Tue Nov 17 22:09:52 2020 IST
gpg:                using RSA key D92E92C12F3E7D3C0BABA73BC674A28337662A96
gpg: Good signature from "Karuppiah Natarajan <karuppiah7890@gmail.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: D92E 92C1 2F3E 7D3C 0BAB  A73B C674 A283 3766 2A96
```

There are also grahical (GUI) applications to help with signing digital files,
you could check those out too :)

Some reference links for you to check out:
https://en.wikipedia.org/wiki/Digital_signature
https://en.wikipedia.org/wiki/Cryptographic_hash_function
https://en.wikipedia.org/wiki/Hash_function
https://www.gnupg.org/
