---
layout: post
title: "Orientation Respectful UINavigationController"
date: 2012-11-16 13:39
comments: false
categories:
---

While working on a project for iOS 6 I ran into an interesting issue with rotations.

I wanted one of my views to rotate, but not others, and since `UIViewController`'s `shouldAutorotateToInterfaceOrientation:` is deprecated in iOS 6 and the docs point at overriding `shouldAutorotate`, I swapped them out. But... `shouldAutorotate` didn't get called. Hrm? A little poking around and I see this in the [iOS 6 release notes](http://developer.apple.com/library/ios/#releasenotes/General/RN-iOSSDK-6_0/_index.html):

> Now, iOS containers (such as UINavigationController) do not consult their children to
> determine whether they should autorotate. By default, an app and a view controller’s
> supported interface orientations are set to UIInterfaceOrientationMaskAll for the iPad
> idiom and UIInterfaceOrientationMaskAllButUpsideDown for the iPhone idiom.

So, because my views are "underneath" a `UINavigationController` their `shouldAutorotate` methods are not getting called, only the parent `UINavigationController` is queried. That seems like an odd behavior to me, but it's easy enough to remedy. A subclass of `UINavigationController` which respects the desired behavior of the top view controller is all that's needed.

This UINavigationController subclass will query it's topmost view controller for desired rotation behavior, unlike the default implementation:

{% gist 4090689 %}

To use, simply include the source in your project, and select `OrientationRespectfulNavigationController` as the class to use in place of each `UINavigationController` you want to have this behavior:

{% img images/posts/2012-11-16-orientationrespectfulnavigationcontroller/IBClassConfig.png 'XCode configuration for UINavigationController class override' 'XCode configuration for UINavigationController class override' %}

NOTE: I'm by no means the first to figure this out. StackOverflow [has](http://stackoverflow.com/questions/12520030/how-to-force-a-uiviewcontroller-to-portait-orientation-in-ios-6) [several](http://stackoverflow.com/q/12777474/397210)  [posts](http://stackoverflow.com/a/12996924/397210) which illustrate the problem and solution.