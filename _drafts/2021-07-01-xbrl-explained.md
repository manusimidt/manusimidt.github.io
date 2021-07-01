---
layout: post
title:  "What is XBRL?"
date:   2021-07-01 18:18:01 +0200
categories: xbrl
tags: xbrl
permalink: /:year-:month/:title
---


> Lately I get a lot of feedback and requests regarding my open source library `py-xbrl`, which makes me very happy. But since I unfortunately can't answer every request and many questions focus on the XBRL standard, I want to give a short overview of the XBRL standard with this blog post.

## Introduction
XBRL (e**X**tensible **B**usiness **R**eporting **L**anguage) is a established XML-based specification used for the **transmission of financial data between two parties**. Transferring financial data between two different parties seems trivial at first glance. However, the huge number of different companies from different industries with different internal accounting systems makes the creation of a common interface a challenge. This is where XBRL comes into play. XBRL allows a wide variety of financial information to be compiled and transmitted. 

A very common use case is the transmission of quarterly reports and annual reports to authorities. For example, every U.S. company (over a certain size) must file quarterly reports (10-Q) and annual reports (10-K) with the SEC using XBRL. These submissions are publicly accessible through SEC EDGAR.


## Putting it all together
![Structure of a XBRL Submission](/assets/img/2021-07-01_full_xbrl_structure.png "Structure of a XBRL Submission")

