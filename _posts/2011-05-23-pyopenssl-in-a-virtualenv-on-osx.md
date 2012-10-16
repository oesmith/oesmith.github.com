---
layout: post
title: PyOpenSSL in a virtualenv on OS X
alias: /post/5762918720/pyopenssl-in-a-virtualenv-on-os-x
---
If you're a Python programmer, no doubt you're now familiar with
[virtualenv][1].  One of its nicest features is `--no-site-packages`, which
isolates your virtual environment from any packages that are already installed
globally.

However, if you're on OS X, using `--no-site-packages` means you can't use the
OpenSSL library that's installed by default.  Trying to `easy_install` or
`pip install` pyopenssl into your virtualenv won't work, since OS X doesn't
ship with OpenSSL headers.

The solution to this little problem is to symlink the system OpenSSL library
into your virtualenv:

    ln -sf /System/Library/Frameworks/Python.framework/Versions/2.6/Extras/lib/python/OpenSSL lib/python2.6/

[1]: http://www.virtualenv.org/
