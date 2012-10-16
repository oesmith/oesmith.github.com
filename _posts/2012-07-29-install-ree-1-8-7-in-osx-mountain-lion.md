---
layout: post
title: Install Ruby Enterprise Edition 1.8.7 in OS X Mountain Lion
alias: /post/28278210238/install-ruby-enterprise-edition-1-8-7-in-os-x-mountain
---
Upgrading to Mountain Lion will wipe your old dev environment, so this is what
you'll need to get back to work on ree-1.8.7:

**1. Install XCode command-line tools.**
Available from the *Preferences > Download* panel in XCode, or as a separate
download from the Apple Developer site.

**2. Install gcc-4.2.**
REE doesn't play well with Apple's LLVM compiler, so you'll need to install the
old gcc-4.2 compiler.  It's available in the homebrew *homebrew/dupes*
repository.

    brew tap homebrew/dupes
    brew install apple-gcc42

**3. Install xquartz.**
The OS X upgrade will also remove your old X11.app installation, so go grab
xquartz from [macosforge][1] and install it (you'll need v2.7.2 or later for
Mountain Lion).

**4. Install ree.**
Remember to add the path to the xquartz X11 includes in CPPFLAGS and the path
to gcc-42 in CC.  Here I'm using rbenv, but the same environment variables
should work for rvm.

    CPPFLAGS=-I/opt/X11/include \
      CC=/usr/local/bin/gcc-4.2 \
      rbenv install ree-1.8.7-2012.02

[1]: http://xquartz.macosforge.org/landing/
