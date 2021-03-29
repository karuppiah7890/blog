---
title: "Bytes #2: An introduction to Consul as a Key Value (KV) Store"
date: 2021-03-29T17:12:35+05:30
---

If you have not heard of [Hashicorp Consul](https://www.consul.io/) before, then this bytes (short) post is going to be interesting!

Consul is an interesting software that has a lot of features, which is a boon and a bane I guess. One of the features it has is - it has a Key Value (KV) store.

A Key Value (KV) store is a kind of data storage software that helps you store data in the form of key value pairs. If you are familiar with objects, hashes, hash tables or dictionary, it's similar to that.

For example, a basic kv store model in your head will look like a table like this -

Key | Value
--- | --- 
thisIsMyKey | thisIsMyValue

When you store information, you store it in key-value pairs, where value is kind of like the main data and key is to just find the data and retrieve the data. So, to retrieve data, you would provide the key as input to the system and get the value as the output

Let's look at this in action in Consul KV store!

To run Consul in your local or any non production environment, simply download the Consul binary from here - https://www.consul.io/downloads

And then run it in your terminal

```bash
$ # check if it's installed
$ consul version
Consul v1.9.4
Revision 10bb6cb3b
Protocol 2 spoken by default, understands 2 to 3 (agent will automatically use protocol >2 when speaking to compatible agents)

$ # run it!
$ consul agent -dev
```

There will be a lot of logs from the program, you can ignore that as of now or else it can be overwhelming to understand it in the beginning itself. Consul is a long running program, it's a server software, which provides Key Value (KV) store feature. Since it's long running server software, the program will keep running until you stop it with Control + C or whatever keyboard shortcut is needed to kill foreground processes / programs

Now in another terminal use the consul program as a client software and try out the Consul KV store!

```bash
$ consul kv put thisIsASampleKey thisIsASampleValue
Success! Data written to: thisIsASampleKey

$ consul kv get thisIsASampleKey 
thisIsASampleValue
```

This is just the surface of the Consul KV store features. I'll talk about other features in another bytes post! :) Feel free to meddle around with the program, check it's help and play with it! :D
