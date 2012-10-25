---
layout: post
title: "The Shadow Knows"
date: 2012-10-25 16:46
comments: false
categories:
---
Adding drop shadows is a nice little UI touch which brings depth, and I like them, assuming they're done subtlety.

That said, I've been playing around with them a bit and have a couple of things to share.

* If your shadow is to be placed on a rectangular layer, use `layer.shadowPath` to increase performance.

TIP: To easily create the rectangular path: `layer.shadowPath = [UIBezierPath bezierPathWithRect:layer.bounds].CGPath;`

See [Apple's CALayer documentation](https://developer.apple.com/library/mac/documentation/graphicsimaging/reference/CALayer_class/Introduction/Introduction.html#//apple_ref/occ/instp/CALayer/shadowPath) where they say:
> If the value in this property is non-nil, the shadow is created using the specified path instead of the layer’s composited alpha channel. The path defines the outline of the shadow. It is filled using the non-zero winding rule and the current shadow color, opacity, and blur radius.
>
>Specifying an explicit path usually improves rendering performance.

* Similarly, using `layer.shouldRasterize = YES;` should increase performance (See the [iPad drop shadow performance](http://www.omnigroup.com/blog/entry/ipad_drop_shadow_performance_test/) post from [The Omni Group](http://www.omnigroup.com/).

* However, be _sure_ to set the `layer.rasterizationScale` appropriately or you are likely to see pixelated content. This can easily be achieved by: `layer.rasterizationScale = [[UIScreen mainScreen] scale];`

So, to recap... drop shadows can add some really nice, subtle, depth and need not be an overt performance hit.

{% codeblock lang:objc %}
CALayer *layer; //Assumed to be initialized somewhere else
layer.shadowPath = [UIBezierPath bezierPathWithRect:layer.bounds].CGPath;
layer.shouldRasterize = YES;
layer.rasterizationScale = [[UIScreen mainScreen] scale];
{% endcodeblock %}