---
layout: post
title: "Frustrations with Xcode Bots"
date: 2015-03-05 16:15 -0700
comments: false
categories: 
---
Truly I want to like Xcode Bots, and I want them to work for my needs, but I got frustrated enough to tweet this not long ago:

> 3:45 PM MT - 3 Mar 2015 ["I’m so done being bitten and frustrated with Xcode Bots. What a steaming pile. &lt;/rant&gt;" -levigroker](https://twitter.com/levigroker/status/572890492603510784)

This frustration was caused by the latest in a long line of frustrations and their workarounds. Specifically, our bots are now spontaneously failing because of this error:

> "Failed to mmap. Could not write data: Invalid argument (-1)"
{% img /images/posts/2015-03-05-frustrations-with-xcode-bots/build_service_error.png 'build service error' 'build service error' %}

*(don't be fooled by the "Fix It" button which doesn't seem to)*

I'm not the only one, as there are others on the [Apple Dev Forums](https://devforums.apple.com/thread/254233) (login required) which are experiencing the same (or very similar) issue. The suggested workaround is to downgrade from Xcode 6.1.1 to 6.1. Yeah, I'm not about to do that, so our CI build is broken, and I'm left with no choice but to go back to [Jenkins](http://jenkins-ci.org). Honestly, that kind of gives me a sense of relief since I know I can get Jenkins to perform all the tasks I need, and I know if our CI build breaks it will be *my* fault and not an external dependency like Xcode Bots, over which I have no control.

I mentioned a long line of frustrations with Xcode Bots, and that's true. I've not taken the time to try to document them all, but here's some off the top of my head:

* In my post [Xcode Bot Keychain Configuration]({% post_url 2015-03-05-xcode-bot-keychain-configuration %}) I mentioned the fact that the bot server does not seem to fetch non-development provisioning profiles, and the steps needed to work around this issue.
* Because of the aforementioned provisioning profile issue, we have _never_ been able to get our Mac app build to work. We always get a provisioning profile mismatch error. We even went so far as to copy the developer cert and private key into the bot's keychain...
* Inexplicably, a CI build will fail, and automatically rebuild, which will then succeed.
* If a build ("integration") succeeds, we get two for the price of one (no idea why... no new commits triggered it).
* The web UI is... gone? In previous builds of the Xcode Bot server there was a web UI which presented the details of each build. This was removed, so any truly useful information must be gathered from Xcode.
* Tracking down build system issues is laborious and cumbersome. The only real source of valuable information is the raw "Build Log" file, which can be viewed from within Xcode, but only after the integration has finished (no "tail -f"-like behavior), but be prepared to wait and keep scrolling as this file loads, slowly, with no UI feedback from your remote build server.
* In general, the internals of the Xcode Bot system are so black box and inaccessible that debugging the process, understanding how it works, or extending it, is a non-trivial time investment which I'd rather use to be working on actual projects.

After all this, I still have high hopes for Xcode Bots, I do want to like them, and I look forward to the day when I can use them for my needs. That day is not today, unfortunately.