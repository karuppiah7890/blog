---
title: "Asynchronous Operations"
date: 2021-01-01T10:42:35+05:30
draft: true
---

Why? For different reasons

- Long background tasks
- No need to get feedback immediately in a synchronous manner
- Plugins / Extensions

Initiating operation in an asynchronous manner

- Message systems

Initiating operation in a synchronous manner

- APIs (HTTP, RPC etc)

If there's no need to check result of the operation, all good.

If there's a need to check result of the operation, there are some
methods.

Different methods to get back status in case one is needed, for an asynchronous
operation for different use cases and constraints and architecture

- Webhook API(s) in the caller system to send the result back to caller. Target
  system invokes the Webhook API(s) with the result.
- Messaging Systems connecting the two systems, through which result can be
  sent back to caller
- Result API in target system to check Result. An API which can be called by
  the caller

Caller refers to the invoker of the operation.

---

Notes:
Concept of hooks and particularly web hooks for asynchronous communication, for
notifications or callbacks.

Polling status from client side vs pushing status from server side :)

Delegate or defer some task to an outside or external system.

Using API to poll vs Webhook API vs events architecture like Kafka messages
