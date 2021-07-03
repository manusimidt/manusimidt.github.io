---
layout: post
title:  "What is XBRL?"
date:   2021-07-01 18:18:01 +0200
categories: xbrl
tags: xbrl
permalink: /:year-:month/:title
---


> Lately I get a lot of feedback and requests regarding my open source library `py-xbrl`, which makes me very happy. But since I unfortunately can't answer every request and many questions focus on the XBRL standard, I want to give a short overview of the XBRL standard with this blog post.

## 1 Introduction
XBRL (e**X**tensible **B**usiness **R**eporting **L**anguage) is a established XML-based specification used for the **transmission of financial data between two parties**. Transferring financial data between two different parties seems trivial at first glance. However, the huge number of different companies from different industries with different internal accounting systems makes the creation of a common interface a challenge. This is where XBRL comes into play. XBRL allows a wide variety of financial information to be compiled and transmitted. 

A very common use case is the transmission of quarterly reports and annual reports to authorities. For example, every U.S. company (over a certain size) must file quarterly reports (10-Q) and annual reports (10-K) with the SEC using XBRL. These submissions are publicly accessible through SEC EDGAR.

## 2 The Taxonomy
Since there is a large number of financial information with different accounting systems (us-gaap, ifrs...), a common language framework must be established prior to the data transfer. In the XBRL context, this common language framework is called a **Taxonomy**. The taxonomy defines a list of concepts that can then be used in the instance document and ensures the integrity and syntactic correctness of the data.
> It has proven useful to create a separate taxonomy for each accounting system - e.g. IFRS, German GAAP (HGB), US GAAP. 

### 2.1 The Taxonomy Schema
The taxonomy schema is the heart of the taxonomy and defines the concepts of the taxonomy. The concepts will later be used by the creator of a financial report to tag certain numbers.
> An example: The US-GAAP taxonomy defines the concept "Cash and Cash Equivalents". Every company that creates a financial report based on us-gaap can use this concept (`us-gaap:CashAndCashEquivalents`) and associate it with a number in their financial report.

### 2.2 Taxonomy Linkbases
Linkbases are individual XML files that bring structure to concepts and link them to additional information. This information can be, for example, user-friendly labels or references to authoritative literature. The linkbases are imported in the taxonomy schema. 
Linkbases can be divided into two main groups; **Relation Linkbases** and **Reference Linkbases**. Relation Linkbases create hierarchical relationships between multiple concepts. The interpretation of these hierarchical relationships is defined by the type of linkbase. Reference linkbases, on the other hand, add resources to concepts. 

#### 2.2.1 Relation Linkbases
![Structure of a relation linkbase](/assets/img/2021-07-01_relation_linkbase.png "Structure of a relation linkbase")

**Calculation Linkbase:** The calculation linkbase 

**Presentation Linkbase:**

**Definition Linkbase:**

#### 2.2.2 Reference Linkbases
![Structure of a reference linkbase](/assets/img/2021-07-01_reference_linkbase.png "Structure of a reference linkbase")

**Label Linkbase:**

**Reference Linkbase:**


## 3 Instance Documents

## 4 Putting it all together
![Structure of a XBRL Submission](/assets/img/2021-07-01_full_xbrl_structure.png "Structure of a XBRL Submission")

