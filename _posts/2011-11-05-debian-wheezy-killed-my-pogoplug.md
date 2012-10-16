---
layout: post
title: Debian Wheezy Killed My Pogoplug
alias: /post/12364561419/debian-wheezy-killed-my-pogoplug
---
After months of running a hybrid sarge/wheezy installation on my Pogoplug (the
wheezy bits needed for [OS X Lion Time Machine support in netatalk][1]), a
power cut forced a reboot.  Unfortunately the poor thing never came back to
life.

A prod with an FTDI cable on the [Pogoplug's serial headers][2] retrieved the
following console grumbles:

    udevd[45]: unable to receive ctrl connection: Function not implemented
    udevd[45]: unable to receive ctrl connection: Function not implemented
    udevd[45]: unable to receive ctrl connection: Function not implemented
    udevd[45]: unable to receive ctrl connection: Function not implemented
    udevd[45]: unable to receive ctrl connection: Function not implemented
    udevd[45]: unable to receive ctrl connection: Function not implemented

It seems a package update had introduced a version of udevd that the poor
Pogoplug's kernel isn't able to support.  The good news is that you don't
really need the debian-installed udevd daemon.  The initrd image that the
Pogoplug boots from has its own, older udevd which is capable enough.

Plugging the pogoplug's root disk into a Linux laptop and disabling the udevd
daemon (insert `exit 0` somewhere near the top of `/etc/init.d/udev`) brought
my Pogoplug back to life, and it'll hopefully mean I don't have to consider a
more expensive NAS for a long time yet.  Yay!

[1]: /post/7897354360/make-timemachine-in-os-x-lion-work-with-debian-squeeze
[2]: http://www.hack247.co.uk/pogo-plug-pink-serial-connection/
