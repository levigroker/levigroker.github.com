---
layout: post
title: "Elegant JSON Pretty Print for BBEdit"
date: 2012-09-07 10:52
comments: true
categories:
---
Working with JSON in BBEdit is great, but reading JSON is greatly enhanced by having it formatted nicely, and BBEdit doesn't have anything (that I've found) which supports pretty printing JSON by default.

A while ago I found this post [http://crisp.tumblr.com/post/2574967567/json-pretty-print-formatting-in-bbedit](http://crisp.tumblr.com/post/2574967567/json-pretty-print-formatting-in-bbedit) which worked great, but I ran into [this elegant little snippet](http://coderwall.com/p/g1cm2g). Inspired, I created a much simpler Text Filter for BBEdit:

{% gist 3667653 %}

(drop this file in your `~/Library/Application Support/BBEdit/Text Filters` directory and then you can select it from BBEdit's Text->Apply Text Filter menu.

## Update
###### (Monday, September 24 2012)
A little less elegant in the script, but this updated version has a nicer output:
{% gist 3777091 %}
(Thanks to Bob Foster for this version)

## Update
###### (Monday, November 3 2014)
Bob Foster passes attribution to [http://ruslanspivak.com/2010/10/12/pretty-print-json-from-the-command-line/](http://ruslanspivak.com/2010/10/12/pretty-print-json-from-the-command-line/)