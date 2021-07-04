---
layout: post
title:  "What is XLink?"
date:   2021-05-25 23:40:32 +0200
categories: xbrl, xml
tags: xbrl, xml
permalink: /:year-:month/:title
---

> This blog post mainly discusses extended links, as they are an important concept for XBRL linkbases.

Widely used for linking resources on the web is the "href" attribute. It provides a simple unidirectional link to another resource and is used, for example, in the anchor tag `<a>` in HTML to create a link to another web address. However, the "href" attribute quickly reaches its limits for more complex problems, which is why the World Wide Web Consortium (W3C) has developed an additional specification, the **XML Linking Language (XLink)**.

With XLink the linking of several elements within as well as between XML documents is possible. Logical relationships between two or more XML elements can be modeled. Thus, in addition to the already known unidirectional links, more complex link structures can also be created. Unidirectional links between two resources are called **simple links** (`xlink:type="simple"`), links to any number of resources are called **extended links** (`xlink:type="extended"`).

The XBRL standard mainly uses extended links, which is why we will focus on them in the following.

When you create an extended link, you can freely choose the name for your link element. In code example below it got the name `<studentLink>`. The only important thing here is the attribute `xlink:type`, which specifies the type of link. 

After creating the link element, all external elements that are to be linked to the link element must first be referenced. This is done with the so-called **locator**. In code example below, the external resource named "student123", which is defined in the schema file "students.xsd", is referenced in this way.
Next, the element to be associated with the first element must be defined. It can either be created locally (in the same file) or referenced remotely (in another file). The first code example defines the firstname and lastname as seperate local resources. The value "resource" of the attribute `xlink:type` indicates that this is a locally defined resource. 

Now that it is clear which elements and information play a role in the link, it must be defined how these are to be linked.
For this the so-called **Arcs** are used. In the code example, this would be the element `<go>`. It connects XLink elements with the `from` and `to` attributes. The elements are identified by the "xlink:label" attribute. It is possible that an Arc element links several elements via the label, as is the case with the two resources in the example below, which both have the label "local".

```xml
<studentLink xlink:type="extended">

  <!-- This locator references a XML element in another document -->
  <locator xlink:type="locator" xlink:label="remote1"
           xlink:href="/students.xsd#student123"/> 
  
  <resource xlink:type="resource" xlink:label="local" 
            xlink:role="first-name">Peter</resource>
  
  <resource xlink:type="resource" xlink:label="local" 
            xlink:role="last-name">Smith</resource>
  
  <!-- This arc connects the locator with the locally defined resources -->
  <go xlink:type="arc" xlink:from="local" xlink:to="remote1"/> 
  
</studentLink>
```

The second code example shows a link between two remotely stored elements. But the basic structure is the same, we now have only two locators instead of one locator and multiple resources.

```xml
<studentLink xlink:type="extended">
  <!-- Both locators reference an XML element in another document -->

  <locator xlink:type="locator" xlink:label="remote1"
           xlink:href="/students.xsd#student123"/> 
  
  <locator xlink:type="locator" xlink:label="remote2" 
           xlink:href="/grades-123.xsd"/>

  <!-- This arc connects the two remotly defined XML elements -->
  <go xlink:type="arc" xlink:from="remote2" xlink:to="remote1"/>
  
</studentLink>
```

### Summary
XLink is a Specification that allows for defining linkings between two XML-elements. These elements can either be **locators** that reference an element in another document, or **resources** that are defined locally. The linking of two or more elements is done with so-called **arcs**.

This blog post focuses mainly on extended links, as they are very important in XBRL linkbases. However, the XML Linking Language contains way more concepts. You can find the original specification [here](https://www.w3.org/TR/xlink11/).

