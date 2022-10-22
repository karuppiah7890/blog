---
title: "Mocking APIs and embedding code snippets for API usage"
date: 2022-10-23T00:39:30+05:30
---

Recently I was checking out [Kong](https://konghq.com)'s Open Source Kong API Gateway and [Kong's documentation on services and routes](https://docs.konghq.com/gateway/3.0.x/get-started/services-and-routes/#managing-services-and-routes) when I stumbled upon [Mockbin][mockbin-website], an interesting service by Kong, and it's open source! mockbin source code is here - https://github.com/Kong/mockbin . According to their website, in short what [Mockbin][mockbin-website] does is - `Mockbin allows you to generate custom endpoints to test, mock, and track HTTP requests & responses between libraries, sockets and APIs.` . You can read more about [Mockbin][mockbin-website] on their website :D

By the way, my exploration didn't end there, I found another service on Mockbin website when I noticed code snippets to use the mock API in the `Send Some Requests` section in a bin web page. It said `Powered by APIembed` at the bottom and it got me curious. I followed the link and found a service called [APIembed][apiembed-website]. It's a service for embedding code in different languages for API usage :D You just need to give an example request in the form of HAR (`HTTP Archive`) and it gives you embedded code for the API usage in different languages :D According to their website, what [APIembed][apiembed-website] helps with is - `Embeddable API Code snippets. Auto-generated code snippets in many programming languages for your website, blog or API documentation` and it's clearly `Made with ♥ at Kong` :D

I loved these small services. By the way, [APIembed][apiembed-website] is open source too! [APIembed][apiembed-website] source code is here - https://github.com/Kong/apiembed , and it uses https://github.com/Kong/httpsnippet under the hood to auto generate code in different languages

I would recommend checking out these services if you work in the area of APIs and would be interested in using these services for your testing, docs etc. Btw, you can self host these services too with the source code that's available :D That's the best part!! I'm sure there are many such services and tools out there, including many open source ones. This one just caught my eye and captured my heart so I'm writing about it. Both Mockbin and [APIembed][apiembed-website] are beautiful :)

[mockbin-website]: https://mockbin.org "Mockbin"

[apiembed-website]: https://apiembed.com "APIembed"
