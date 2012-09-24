---
layout: post
title: "Override Deprecated API Warning"
date: 2012-05-07 15:58
comments: true
categories:
---
Sometimes I need to make a call to a deprecated API (for DEBUG builds only, of course) and don't want to see the warning. This little preprocessor tip will hide that warning:
{% gist 2630702 %}