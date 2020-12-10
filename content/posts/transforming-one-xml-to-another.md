---
title: "Transforming one XML to another"
date: 2020-12-10T19:40:35+05:30
draft: true
---

Recently I had this task of parsing an XML file data into an object. The
challenge was that there were different kinds of XML files - different
structures, and we wanted dynamic mapping - where in we could use different
configurations and map the XML data to the same object - same class basically.

There are different ways to go about it. One could use XPath as configuration
and map it to the object, or maybe convert the XML content into a different
format like JSON and then parse the JSON with some dynamic configuration and
map it to the object.

There's one more way too that our team is experimenting, which is, betting on
the standards - XSLT. So, the plan is, to convert the different XML files with
different structures into a standard XML using XSLT configurations, and then
using the standard XML to create the object. It's called the standard XML as
it's a standard for us, based on the object/class. The standard XML has a direct
mapping to the class and can be parsed into the class object with ease using a
standard XML parser. So, we just need to create multiple XSLT transformations
for differently structured XML files to convert them into our standard XML
file.

In this post we are going to see how a basic example of how to convert an XML
to another XML, and also how to parse the XML into an object of a particular
class

References:
Oracle post -
https://docs.oracle.com/javase/tutorial/jaxp/xslt/transformingXML.html

https://www.javatpoint.com/jaxb-unmarshalling-example

Any other posts about XSLT, XML

Some more ideas:
Such transformations are pretty usual in the world of software, especially when
it comes to integrations and also in projects like data projects where a lot of
data is collected and processed - ETL (Extract, Transform, Load).

Similar to converting one XML to another XML based on a configuration, there are
other such transformers too. For example, convert one JSON to another JSON using
a configuration - more like JSON transformation. ;)
