---
layout: post
title: "My Bank Holiday Project: Tweet GP"
---
This is [my latest project][1] - a simple little web app that plays back
twitter feeds from F1 races.  It's just like a Sky+ box, but for F1 tweets.  If
you miss the race, you can still watch it with all the best commentary from a
hand-picked selection of the Twitterati.

![Introducing Tweet GP](http://media.tumblr.com/tumblr_lkolvtnmOH1qb5o7n.png)

Events go live on Tweet GP the moment the coverage starts on the BBC, so even
if you're only minutes behind the action, you don't have to miss a thing.

![Where the action's at](http://media.tumblr.com/tumblr_lkolzp56Sy1qb5o7n.png)

It should work on all modern browsers.  I've tested in on the current versions
of Chrome, Firefox and Safari (both on the desktop and on iOS).  It also works
on IE8 (although it doesn't quite look as sexy).  If anyone can test on IE9 or
Opera, I'd be grateful for feedback.

If all you want to do is get time-shifted tweets,
[go straight to tweetgp.com now][1].

If you're of a *very* geeky persuasion, feel free to read on and get the back
story of how it was all built.

A few weeks ago I was sat at home watching the Malaysian Grand Prix.  I had
woken up at 8am, which was too early for comfort on a Sunday morning and,
crucially, two hours too late to watch the coverage on the BBC live.

Thankfully, like the rest of the civilised world, we have a very smart HUMAX
set-top box that time-shifts TV for us, so I was able to watch the TV coverage
from the start (in HD – thank you, BBC).

However, I had to put my iPhone down and stay away from twitter for the
duration of the race in case I found out the result.  This meant missing out on
the insight and comic talents of several people I follow who usually make the
race a much more enjoyable event.

Now, being a hacky kind of guy, I decided I could solve this problem with code
(and at the same time learn something new).  In time for the Chinese Grand
Prix, I’d cobbled up a bit of [Node.js][2] code that connected to the 
[Twitter streaming API][3], and sucked up a live stream from all the
interesting F1 people, pushing it into a local [Redis][4] instance.

Using that lot, I was able to get out of bed at a sensible time to watch the
action in Shanghai, without missing out on the comedy stylings of the likes of
[@sniffpetrol][4].  Much rejoicing.

Since then, I’ve spent my bank holiday weekends packaging that lot up in a
shiny HTML5 box.  Everything on the client side is done without any plugins or
proprietary extensions.  As such, the code should work on all modern browsers
(I’ve tested it on Chrome 11, Safari 5, Firefox 4, IE 8 and iOS 4.3).

I’m using [express][6] to serve up the front-end pages.  All the code’s written
in [coffeescript][7], and the styling’s done in [sass][8]. Live tweets get
pushed straight to the clients using Redis pubsub and [socket.io][9].  The
server lives in the [Rackspace Cloud UK][10].

And that's it! Well done if you've read this far. I think next I'll be signing
up for a go on Twitter's Site Streams beta API, and seeing if I can generalise
this a bit.  Fancy having a recording of all your tweets ready to playback for
any TV program?  Watch this space!

[1]: http://tweetgp.com
[2]: http://nodejs.org
[3]: http://dev.twitter.com/pages/streaming_api
[4]: http://redis.io
[5]: http://twitter.com/sniffpetrol
[6]: http://expressjs.com
[7]: http://jashkenas.github.com/coffee-script/
[8]: https://github.com/visionmedia/sass.js
[9]: http://socket.io
[10]: http://www.rackspacecloud.co.uk
