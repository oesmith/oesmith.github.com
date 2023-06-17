---
layout: post
title: Subversion and Visual C++
---
Despite mostly using Java for the work I’m doing at the moment, there’s still
some call for a bit of C++. Visual Studio 2005\* is still the daddy
of all IDEs for developing in C++ as far as I’m concerned, but all the
intermediate cruft that it generates doesn’t really need checking in to my
Subversion repository.

You can cut down the extraneous crap by excluding all `debug` and `release`
directories, and all `.ncb`, `.suo`, `.vcproj.USERNAME.user` and `.aps` files.

*\* Yes, I know 2008 is available, and I’ve even got it downloaded from MSDN,
but I’ve not got around to installing it yet...*
