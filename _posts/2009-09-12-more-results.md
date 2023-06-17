---
layout: post
title: More results
---
Since the last update, I’ve been concentrating on stopping character shapes
from bleeding into other elements of the image – mostly it’s been the edges of
number plates that have been getting in the way. I’ve used a hough transform to
find the number plate edges and eliminate them from the thresholded image
before component labeling. I’ve also discarded some of the components based on
a bit of filtering after labeling.

![More results](https://farm3.static.flickr.com/2493/3911368101_724af50d25.jpg)
