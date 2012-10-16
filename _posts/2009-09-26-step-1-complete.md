---
layout: post
title: "Step 1: complete"
alias: /post/470849018/step-1-complete
---
The clustering algorithm has been tuned and it’s working reliably—even on all
sorts of odd foreign plates!

On top of that, I’ve added a de-skewing and extraction function. These are the
results extracted from the previous image.

![An extracted plate](http://farm3.static.flickr.com/2497/3955585309_eb4a8b0ab0.jpg)
![Another extracted plate](http://farm3.static.flickr.com/2579/3955585331_3b9ee5ea10.jpg)

I’m happy with the speed too – these two were extracted in 0.2s. The only
downside at the moment is that the clustering algorithm is roughly O(n^2) in
terms of the number of contours, so on busier images it can take a second or
two to do the clustering stage (and it’ll produce an occasional false
positive).
