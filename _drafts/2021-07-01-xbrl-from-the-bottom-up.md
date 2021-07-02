---
layout: post
title:  "XBRL from the bottom up"
date:   2021-07-01 18:18:01 +0200
categories: xbrl
tags: xbrl
permalink: /:year-:month/:title
---


> With this blog post I will try to explain XBRL as simple as possible using a problem-solving oriented approach. I will show the challenges in transferring financial data and explain how XBRL solves them. **For a more detailed and traditional explanation, please check out this blog post: [What is XBRL?](/2021-07/xbrl-explained).**

<div style="text-align:center; margin: 10px"><img src="/assets/img/2021-07-01/l1.png" /></div>


Ok, so our goal is to transfer some financial data. Let's say we want to tell the world that we generated 15.7 Million USD in Revenue in 2020. We also want our revenue report to be machine readable, no oldschool pdf's! How can we encode and transfer our financial data?

The common approach would be to create another markup language, perhaps YAML 2.0 😉. On the other hand, it might also be smart to simply use an already existing and established standard. How about the most famous markup language in the world - XML?

In XML, we could encode our data as follows:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<report>
  <revenue unit="usd" from="01.01.2020" to="31.12.2020">15700000</revenue>
</report>
```

<div style="text-align:center; margin: 10px"><img src="/assets/img/2021-07-01/l2.png" /></div>

Great, our data point is now machine-readable and encoded in a widely established standard. Now we can send out our report and tell the world how much profit we made, right?

Not so fast. There are a couple of issues with the report: 
- The recipient of the xml file does not know **which company** to congratulate for the $15.7M in revenues.
- In an annual report, we will have hundreds of financial data points from the same period. We would have to **redefine the period and the company** (to which the number belongs) for every data point.
- The unit of this example is quite simple. But how do we deal with financial data like "earnings per share" which would have the **unit "USD per share"**?

Ok, lets make some improvements to our xml report:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<report>
  <unit id="usd">
    <measure>USD</measure>
  </unit>

  <context id="2020FY">
    <startDate>01.01.2020</startDate>
    <endDate>31.12.2020</endDate>
    <entity>865985</entity> <!-- This is our company identifier that the auditors gave us-->
  </context>

  <revenue unit="usd" context="2020FY">16000000</revenue>
</report>
```

We have now solved the problems from above relatively well, can we send the report now? Unfortunately not quite..

We are obviously not the only company in the world. If everyone did what we do, the agencies would have a really bad time processing **hundreds of slightly different reports**. We need a way to define the general structure of the report so that all financial reports have the same structure.
Fortunately, XML provides an excellent way to specify the structure of an XML file with XML schemas. 


This is where the **XML Specification** comes into play. Fortunately, some clever minds have already created it, so we'll just use it here for now and not go into the schema file itself.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<xbrli:xbrl
  xmlns:xbrli="http://www.xbrl.org/2003/instance"
  xmlns:iso4217="http://www.xbrl.org/2003/iso4217">

  <xbrli:unit id="usd">
    <xbrli:measure>iso4217:USD</xbrli:measure>
  </xbrli:unit>

  <xbrli:context id="2020FY">
    <xbrli:entity>
      <xbrli:identifier scheme="http://www.sec.gov/CIK">0000320193</xbrli:identifier>
    </xbrli:entity>
    <xbrli:period>
      <xbrli:startDate>2016-09-25</xbrli:startDate>
      <xbrli:endDate>2017-09-30</xbrli:endDate>
    </xbrli:period>
  </xbrli:context>  

  <revenue unitRef="usd" contextRef="2020FY">16000000</revenue>
</xbrli:xbrl>

```


<div style="text-align:center; margin: 10px"><img src="/assets/img/2021-07-01/l3.png" /></div>

By the way, the XML file we are creating is called **Instance Document** in the XBRL jargon 😄. It holds all our financials that we want to publish.

<div style="text-align:center; margin: 10px"><img src="/assets/img/2021-07-01/l4.png" /></div>

So to summarize, the basic structure of our Instance Document (our XML report) is now defined by the XBRL Specification. We have introduced contexts that contain information about the timeframe and company a number is related to. We have also introduced unit-elements that define the unit that belongs to our number. Both units and contexts can be referenced many times in our Instance Document. 

But one thing is missing. Currently no one tells us which financial information we can encode in our document. Currently we only have one `<revenue>` element. Can we add another called `<cash>`? This is regulated by a so-called **Taxonomy**. The taxonomy defines the different (accounting) **concepts** that can be used in the Instance Document. It also contains additional information about the relationships between multiple concepts and additional resources for concepts (e.g. labels), but I won't go into that in this blog post.


#### Our Instance Document:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<xbrli:xbrl
  xmlns:xbrli="http://www.xbrl.org/2003/instance"
  xmlns:iso4217="http://www.xbrl.org/2003/iso4217"
  xmlns:us-gaap="http://fasb.org/us-gaap/2020-01-31">

  <xbrli:unit id="usd">
    <xbrli:measure>iso4217:USD</xbrli:measure>
  </xbrli:unit>

  <xbrli:context id="2020FY">
    <xbrli:entity>
      <xbrli:identifier scheme="http://www.sec.gov/CIK">0000320193</xbrli:identifier>
    </xbrli:entity>
    <xbrli:period>
      <xbrli:startDate>2016-09-25</xbrli:startDate>
      <xbrli:endDate>2017-09-30</xbrli:endDate>
    </xbrli:period>
  </xbrli:context>  

  <us-gaap:Revenue unitRef="usd" contextRef="2020FY">16000000</us-gaap:Revenue>
</xbrli:xbrl>
```

#### The Taxonomy:
```xml
<xsd:schema 
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xbrli="http://www.xbrl.org/2003/instance">

  <xsd:element id="us-gaap-Revenue" name="Revenue"
    nillable="true" substitutionGroup="xbrli:item"
    type="xbrli:monetaryItemType"
    xbrli:balance="credit" xbrli:periodType="duration"/>
  <!-- Many more xml elements (Concepts)... -->
</xsd:schema>
```



<div style="text-align:center; margin: 10px"><img src="/assets/img/2021-07-01/l5.png" /></div>


## Summmary
To summarize: Our goal is to transfer financial data. Each financial data point is called a **Fact**. To each fact belongs a **Context** that stores information about the date or timeframe as well as the company the fact belongs to. Most facts also have a **Unit** attached to them. To transfer our facts we start by creating an XML document. This XML document is called **Instance Document**. The structure of our Instance Document is defined by the **XBRL Specification**. The different types of facts i.e: (Assets, Liabilities, CashAndCashEquivalentsAtCarryingValue) we are allowed to file are defined in the **Taxonomy**. 

This blog post is written with the goal of providing the most easily digestible technical introduction to XBRL. However, it is therefore somewhat imprecise in some places and/or omits important elements. A more detailed description can be found in my Blog Post [What is XBRL?](/2021-07/xbrl-explained).