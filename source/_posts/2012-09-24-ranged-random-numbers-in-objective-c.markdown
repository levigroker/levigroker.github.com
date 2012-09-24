---
layout: post
title: "Ranged Random Numbers in Objective C"
date: 2011-12-05 12:47
comments: true
categories:
---
Sounds pretty basic, and it is, but like many things there are lots of ways to do it. This is my new favorite:

{% codeblock lang:objc %}
u_int32_t arc4random_uniform(u_int32_t /*upper_bound*/) __OSX_AVAILABLE_STARTING(__MAC_10_7, __IPHONE_4_3);
{% endcodeblock %}

Based on [discussions on StackOverflow](http://stackoverflow.com/a/7082580/397210) and my own testing.

This assumes you're on iOS 4.3 or later, however.