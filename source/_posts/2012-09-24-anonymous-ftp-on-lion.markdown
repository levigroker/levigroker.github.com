---
layout: post
title: "Anonymous FTP on Lion"
date: 2011-11-07 13:14
comments: true
categories:
---
Start:
------
{% codeblock lang:sh %}
sudo dscl . -create /Users/ftp
sudo dscl . -create /Users/ftp NFSHomeDirectory /Users/labrown/Public/ftp
sudo launchctl load -w /System/Library/LaunchDaemons/ftp.plist
{% endcodeblock %}
Stop:
-----
{% codeblock lang:bash %}
sudo launchctl unload -w /System/Library/LaunchDaemons/ftp.plist
sudo dscl . -delete /Users/ftp
{% endcodeblock %}
