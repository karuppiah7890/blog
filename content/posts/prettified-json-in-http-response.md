---
title: "Prettified JSON in HTTP Response"
date: 2020-04-19T18:16:33+05:30
---

I was randomly thinking about JSON, jq tool and the tool's ability to
prettify JSON by indenting with spaces and it's ability to remove
spaces too.

Suddenly for a second I was thinking why HTTP responses should not
have spaces? If they had spaces they would look so cool right? 😂

In a moment I got the answer - yeah. You got it right. Prettifying the
JSON with indentation will mean - add spaces. Space characters.
Characters require space, occupy space. So, your HTTP response is going
to have more data - more weight. More data means the client now has to
download more data. Given many have very very low network bandwidth,
they don't need to download more data, and why would they? The computer
can understand the JSON with no spaces too 😅🙈 so yeah. That's the
reason

The reason is the same for all those obscure JavaScript scripts that
we download. Why is it obscure? Read [here](/blog/posts/obscure-javascript/)
