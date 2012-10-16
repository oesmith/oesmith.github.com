---
layout: post
title: Running openoffice/libreoffice as a service
alias: /post/5670961061/running-openoffice-libreoffice-as-a-service
---
So, you're trying to use JODConverter or unoconverter, you've set up openoffice
to launch as a service in the background somewhere, but it's not going
anywhere, and all you're seeing in your logs is

    creation of executable memory area failed: Permission denied
    creation of executable memory area failed: Permission denied
    creation of executable memory area failed: Permission denied
    creation of executable memory area failed: Permission denied

The solution, my friend, is to make sure that the openoffice process gets a
writable path in its HOME environment variable.  In my case, the supervisord
config entry looks like this:

    [program:openoffice]
    command=/usr/lib64/openoffice/program/soffice.bin -headless -nofirststartwizard -accept="socket,host=localhost,port=8100;urp;StarOffice.Service"
    environment=HOME="/tmp"
    user=www-data

Hopefully this will save someone else the hour of fruitless hackery that I've
been through to get to this point.
