---
layout: post
title: "GRKImageCrop"
date: 2015-04-28 13:53:26 -0600
comments: false
categories: 
---
A UIImage category which provides "visible" pixel cropping capabilities.

A given image can be rectangularly cropped based on calculated insets to "visible" pixels.
Visible pixels are those whose alpha component are greater than or equal to a given alpha
threshold.

Available on github: [https://github.com/levigroker/GRKImageCrop](https://github.com/levigroker/GRKImageCrop)

{% img /images/posts/2015-04-28-grkimagecrop/Demo.gif 'demo' 'demo' %}

### Installing

If you're using [CocoPods](http://cocopods.org) it's as simple as adding this to your
`Podfile`:

	pod 'GRKImageCrop'

otherwise, simply add the contents of the `GRKImageCrop` subdirectory to your
project.

### Documentation

To use, simply import `UIImage+GRKImageCrop.h`:

    #import "UIImage+GRKImageCrop.h"
    
Then you can use the category to create a cropped image from a given image:

	[image cropImageBelowAlphaThreshold:0.0f completion:^(UIImage *croppedImage, NSError *error) {
		if (image)
		{
			//Use croppedImage
		}
		else
		{
			//Hanlde error
		}
	}];

The only expected error would be memory related.

Additional documentation is available in `UIImage+GRKImageCrop.h` and example usage
can be found in the `GRKImageCropTestApp`.

#### Disclaimer and Licence

* Inspiration was taken from:
    * [https://developer.apple.com/library/mac/qa/qa1509/_index.html](https://developer.apple.com/library/mac/qa/qa1509/_index.html)
    * [http://stackoverflow.com/a/1262893/397210](http://stackoverflow.com/a/1262893/397210)
    * [http://stackoverflow.com/a/12617031/397210](http://stackoverflow.com/a/12617031/397210)
    * [https://github.com/wrep/UIImage-Trim](https://github.com/wrep/UIImage-Trim)
* I have made use of image cropping code from [http://stackoverflow.com/a/25293588/397210](http://stackoverflow.com/a/25293588/397210)
* This work is licensed under the [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/).
  Please see the included LICENSE.txt for complete details.
