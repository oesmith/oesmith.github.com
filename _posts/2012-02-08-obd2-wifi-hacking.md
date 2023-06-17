---
layout: post
title: OBDII Wi-Fi Hacking
---
There are plenty of these OBDII WiFi modules advertised for sale on eBay.  I've
recently bought a RenaultSport Clio which has an OBDII port, so I picked up one
to experiment with.

![OBDII Wi-Fi Module](http://media.tumblr.com/tumblr_lz3c6x9SzE1qb5o7n.jpg)

This little gadget plugs into the OBDII port in the Clio (which is hidden
behind a removable panel just below the ignition card key slot).  It creates an
ad-hoc WiFi network and listens on a TCP port for client connections.

The chip that does all the hard work of connecting to the car's onboard
computer(s) is an ELM327-compatible, which means it speaks a simple ASCII
protocol.  Control messages are sent to the chip using old-school AT commands
(like old-school modems), and OBD comms use simple hex strings.

Here's a quick example:

    > ATZ
    ELM327 v1.4
    0100
    41 00 9E 3E B8 11
    0120
    41 20 80 00 00 00
    0105
    41 05 48

The `ATZ` command resets the interface chip (and displays its ID).  The rest of
the commands all request data from the ECU using OBD mode 1.

`0100` and `0120` enumerate the [OBD PIDS][1] supported by my car.  The data
returned is a header (`41 00` / `41 20`) followed by the actual data as a
bit-encoded set of flags.

`0105` queries the ECU for the current engine temperature (which, in this case,
returns a temperature of 32°C).

I'm going to carry on hacking with this module, and I'm hoping to build a natty
little iOS telemetry app to use with it.  Stay tuned for further blog posts as
I learn more!

[1]: http://en.wikipedia.org/wiki/OBD-II_PIDs#Standard_PIDs
