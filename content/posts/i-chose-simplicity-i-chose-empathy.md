---
title: "I Chose Simplicity. I Chose Empathy"
date: 2020-05-20T18:50:39+05:30
---

Recently I was writing this code for a JSON REST API. We were adding an extra
field to our API structure, for a resource. For the sake of simplicity let's
assume we were building an API to store rich text in our database. The size of
this rich text was small, like 500 characters, that's it. And let's say we came
up with some simple solutions on how to store the rich text. And finally, the
below is what we decided the structure would look like

```json
{
    "textContents": [
        {
            "type": "PLAIN_TEXT",
            "plainText": "hi, what's up? check this link "
        },
        {
            "type": "LINK_TEXT",
            "linkInfo": {
                "link": "https://distinguisheddevs.com/",
                "id": "2148",
                "title": "Distinguished Devs",
                "previewImage": "https://distinguisheddevs.com/static/media/logo_transparent_bg.3ef148d2.png"
            }
        }
    ]
}
```

So, if you notice, we have different kinds of text. For showing the text in the
view, we need to concatenate them and then render it in our view.
So, the above is kind of like a heterogeneous array. Since, the array elements
have different keys. A plain text element will have `PLAIN_TEXT` type and have
only `plainText` key but no `linkInfo` key. 

We used golang for building our API. So, how would one code this in golang?
I looked at some articles related to this. 

"JSON polymorphism in Go"
https://medium.com/@alexkappa/json-polymorphism-in-go-4cade1e58ed1

"From Java to Golang: Unmarshaling Polymorphic JSON"
https://stackoverflow.com/questions/59080215/from-java-to-golang-unmarshaling-polymorphic-json

It seemed like there are ways to do it, where we we use different types to
deserliaze each element in the array, based on it's type. This is what they
called as polymorphism in JSON

We first actually did an implementation where we considered the array kind of
like a homogeneous array. Each element is of the same type, containing all the
fields of the different type of elements, in this case - 2. We just deserliazed
it in a single line with `json.Unmarshal()` and nothing else. And then while
processing, used if statement to find the type of the element and then
accordingly use the fields. To give a gist, based on the example, this is what
the types looked like -

```go
type TextContent struct {
	ContentType string `json:"type"`
	PlainText   string
	LinkInfo    LinkInfo
}

type LinkInfo struct {
	Link         string
	ID           string `json:"id"`
	Title        string
	PreviewImage string
}

type TextContents []TextContent
```

I just had this one nagging thing. I wanted to make sure that the design is
such that one can only access fields based on the type. Or else it's just
bringing in extra fields and mixing them up and possibly creating issues at the
design level. As you can see above, `TextContent` has both the fields `LinkInfo`
and `PlainText`, but actually only one of them would be populated. This also
meant that some amount of memory would be wasted as the default value of a
string is empty string and for a struct is empty struct, so when one of them
is present, other one's just there, an empty default value, taking up memory.
And if someone uses up the wrong field while accessing the contents, without
doing it based on the content type, then it would be blunder. Tests can help,
yes, but still, that's like design level issue. Mixing up things.

And I didn't want this. Something just didn't look right. I didn't want to put
it all together. Which meant different types for different kinds of elements.
And that's what the blogs about JSON polymorphism spoke about. We then checked
out what's the difference in code and also tried to understand the code.

If you notice the code in

https://medium.com/@alexkappa/json-polymorphism-in-go-4cade1e58ed1

There was just a lot of code, interface - of course, as we were dealing with an
array of heterogeneous types. And code also had custom `UnmarshalJSON` and
`MarshalJSON` methods. I was like "What?" and I was thinking about bringing
this implementation in. And then I realized how much code change we had to do
for this simple thing that I wanted to do. In the mean time, I was also thinking
how complex it made the whole code, and was wondering if anyone would
understand what all that code meant. They will only understand if you put
docs and show the sample JSON, along with the custom serializer and
deserliazer methods. And I hated complex too. Also, I also recalled at this
moment that we were going to give this codebase to another team, for them to
take it forward, build features and manage. I mean, it happens to almost any
software project I guess - project being handed over from one team to another
team. So, you would want to make sure that they can read the code and
understand it, right? And also help with docs and share some knowledge through
knowledge transfer sessions. Now, even if the project is not handed to another
team, it's very possible that a person who wrote the code may leave the team,
the team should still be able to read code. Same goes for yourself too! You
write the code today. After a few months, you get back to that code, you
forget about it and it looks gibberish - imagine it - even when you can't
understand the code you wrote. I mean, this the main reason why people
request others to write code others can read and understand. 

I realized that the whole code that I wanted to write based on the blog, looked
quite a bit complex. I mean, the pro I thought was - well, when one iterates
through the array, they will only look at the correct fields related to that
type. I wanted that differentiation. I wanted that separation of concern. But
the big con was readability.

After a few moments, I realised that if we wrote the compelex, people would
struggle to uderstand something very very simple. So, I decided to choose
simplicity and empathy over perfectionism and complexity. I do feel bad about
the design. Also, mind you, some more cons to this design are - when more
different types of elements come into the array, more different fields get
added to the type and also, beware of the size of the array or the whole data.
If it really grows, it's not such a good idea to do it the simple way we did it.
May be a different approach could be thought about, which does NOT need
heterogenous array elements ;) That could be the main problem that I can see!

I wanted to share this to show that sometimes you gotta try to keep it simple.
Simple is hard, as you can see above, that was not a perfect simple, it still
has some cons. A perfect simple is hard to achieve, but I think it's usually
possible if you put your energy into it! :)
