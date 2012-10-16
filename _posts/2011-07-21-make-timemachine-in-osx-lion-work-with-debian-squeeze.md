---
layout: post
title: Make TimeMachine in OS X Lion work with Debian Squeeze (stable) netatalk servers
alias: /post/7897354360/make-timemachine-in-os-x-lion-work-with-debian-squeeze
---

All you need to do to get TimeMachine running again is to install the netatalk
package from unstable.  It's a really simple process.  First add the wheezy
(testing) repository to your apt configuration.

    # /etc/apt/sources.list
    # stable repo
    deb http://cdn.debian.net/debian squeeze main

    # new testing repo
    deb http://cdn.debian.net/debian wheezy main

Then give your squeeze repo a higher priority to your wheezy one (to prevent
wheezy packages being accidentally installed when they're not wanted).

    # /etc/apt/preferences.d/priorities
    Package: *
    Pin: release a=squeeze
    Pin-Priority: 700

    Package: *
    Pin: release a=wheezy
    Pin-Priority: 650

Now install netatalk, making sure to tell apt-get to get it from the wheezy
repo.

    sudo apt-get install netatalk/wheezy

And remember, if you've added an afpd service description to your avahi-daemon
configuration, remove it and restart netatalk (because netatalk 2.2beta will
register itself automatically with avahi).

Happy Lion-ing!
