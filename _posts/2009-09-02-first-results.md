---
layout: post
title: First results
---
I’ve been fiddling with OpenCV on-and-off in my spare time for a few weeks now,
and I’m starting to get a feel for the challenges involved in number plate
recognition. There’s plenty of papers and background material out there to
[keep me reading for months][1].

The simple approach as used by a number of researchers is to concentrate on
thresholding the image (sometimes many levels of thresholding), and then doing
a bit of connected component labelling to find candidate regions of the image
to extract and pass to a second-stage character recognition algorithm.

Here’s my first attempt. Source image, then thresholded and labelled image.

![Source image](https://farm3.static.flickr.com/2470/3885477078_cfc0d6bdfe.jpg)

![Thresholded and labelled image](https://farm3.static.flickr.com/2441/3884666955_254d030c5d.jpg)

[1]: https://delicious.com/olly.smith/ANPR
