---
layout: post
title: "Give me back my ~/Library (!)"
date: 2012-09-07 10:39
comments: true
categories:
---
As a developer I need access to my Library directory, but Apple keeps hiding it from me with every OS update. Finally tired of manually typing `chflags nohidden ~/Library/` every time it disappeared, I created a simple LaunchAgent to do this for me every time I log into my machine.

Check it out:

{% gist 3667585 %}

As an aside, I'll take this space to plug [Lingon](http://www.peterborgapps.com/lingon/) (I have no affiliation), which is a nice GUI tool for managing LaunchAgents and saves you from editing XML and dropping to the command line to talk to `launchd`.