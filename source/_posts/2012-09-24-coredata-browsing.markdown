---
layout: post
title: "CoreData Browsing"
date: 2012-03-15 17:01
comments: true
categories:
---
Found a handy tool for browsing core data on the simulator. It's not very polished, but it allows you to perform fetch requests with predicates and get back results, which helps immensely when trying to understand what's happening inside your app's data at runtime.
Check it out:

[http://atastypixel.com/blog/browsing-core-data-databases-using-f-script/](http://atastypixel.com/blog/browsing-core-data-databases-using-f-script)

I also found this answer on SO which helped me get bootstrapped by giving the location of the two needed files in the simulator:

[http://stackoverflow.com/a/5998072/397210](http://stackoverflow.com/a/5998072/397210)

Happy CoreData spelunking!

P.S. [@tomhoag](https://twitter.com/tomhoag) suggested this [https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager/](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager) as well... looks handy!

## Update
###### (Monday, September 24 2012)
Since the original post, I've discovered [Core Data Editor](http://itunes.apple.com/us/app/core-data-editor/id403025957?mt=12) which is really great and I recommend it. However, the fscript approach allows one to perform predicate based fetch requests which is not something the Core Data Editor supports (at this time?) and is invaluable, so having both tools around is still handy.
