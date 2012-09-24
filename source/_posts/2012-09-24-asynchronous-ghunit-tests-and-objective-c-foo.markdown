---
layout: post
title: "Asynchronous GHUnit Tests &amp; Objective C Foo!"
date: 2012-01-19 17:33
comments: true
categories:
---
[GHUnit](https://github.com/gabriel/gh-unit) is quite useful for running and reporting tests (unit or otherwise) for iOS projects, and I've been using it for a while with good results. Recently, however, I found as I wrote more and more integration-style tests with a remote HTTP service I found the code getting to be a pain to write and maintain.
There were two reasons for this:

#### GHAssert variants fail with an Exception, and therefore other tests do not continue to execute.
This was a real drag, since the assert macros are pretty handy, but I want all my tests to run, even if some fail (*gasp* I know!).

So, I ended up replacing the GHAsserts with a simple conditional, which doesn't feel as clean, but navigates around the issue.

#### Selector names became copy and paste heavy.

Since I'm making notification calls like `[self notify:kGHUnitWaitStatusSuccess forSelector:@selector(testAsynchronousOperation)];` from within the same method it was additional grunt work to copy the selector name to all the places I needed a reference to the selector.

I thought of adding a `SEL mySelector = @selector(foo);` to each method, which would cut down on the copy & pasting, but that just didn't seem clean to me.

I discovered there is an Objective C variable like `self` called `_cmd`[*](http://www.google.com/search?client=safari&rls=en&q=objective+c+_cmd&ie=UTF-8&oe=UTF-8) which is a reference to the current selector(!). That's cool, and simplifies code like my testing code a lot.

#### Sample

For the specific kinds of test cases I was writing, here's an example which shows both of these issues resolved:
{% gist 1643797 %}