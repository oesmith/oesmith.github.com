---
layout: post
title: Netatalk on an Ubuntu virtual machine
alias: /post/470778673/netatalk-on-an-ubuntu-virtual-machine
---

I tend to develop on a MacBook running OS X Leopard. In order to keep my main
Leopard system clean and tidy, I use Parallels desktop to run my development
environments in virtual machines. Parallels is jolly good at sharing data with
Windows virtual machines, but a bit lacking when it comes to Linux. Hence I’ve
started using netatalk on all my Linux virtual machines to access their disk
drives from OS X.

Lots has been written about what it takes to install and tweak netatalk to get
it to talk happily with Leopard, so I won’t go into the problems here. All you
need to know should be found at the following links.

* [[hellinga.org]: Secure passwords with Netatalk][1]
* [Sakuzaku – Essays – Making netatalk Work on Debian with Leopard][2]
* [damontimm.com How to: Install Netatalk (AFP) on Ubuntu with Encrypted Authentication][3]

The process below is an amalgamation of all the instructions from those pages,
put together to form the process I go through to set up netatalk on my virtual
machines.

Firstly, one needs to download the netatalk source, install all of its
dependencies and build it with ssl enabled.

    $ mkdir -p ~/src/netatalk
    $ cd ~/src/netatalk
    $ sudo aptitude install devscripts cracklib2-dev dpkg-dev libssl-dev
    $ apt-get source netatalk
    $ sudo apt-get build-dep netatalk
    $ cd netatalk-2.0.3
    $ sudo su
    # DEB_BUILD_OPTIONS=ssl dpkg-buildpackage -us -uc
    # debi
    # echo "netatalk hold" | dpkg --set-selections
    # exit


That should have build and installed the netatalk package, and told the debian
packaging system to not replace it if a new version appears in the online
repositories.

The only netatalk service I’m interested is the file sharing one, so the rest
can be turned off in /etc/default/netatalk:

    # Set which daemons to run (papd is dependent upon atalkd):
    ATALKD_RUN=no
    PAPD_RUN=no
    CNID_METAD_RUN=no
    AFPD_RUN=yes
    TIMELORD_RUN=no
    A2BOOT_RUN=no

Finally, to enable zeroconf/bonjour discovery of the shares on the system,
install avahi-daemon and add a service definition to 
`/etc/avahi/services/afpd.service` to advertise the afp service.

    <?xml version="1.0" standalone=‘no’?><!–*-nxml-*–>
    <!DOCTYPE service-group SYSTEM "avahi-service.dtd">
    <service-group>
      <name>%u</name>
      <service>
        <type>_afpovertcp._tcp</type>
        <port>548</port>
      </service>
    </service-group>

And that’s it! You should now be able to reliably mount your home directory on
the Linux virtual machine from OS X.

[1]: http://www.hellinga.org/index.php?id=2&tx_ttnews%5Btt_news%5D=4&tx_ttnews%5Byear%5D=2007&tx_ttnews%5Bmonth%5D=05&tx_ttnews%5Bday%5D=26&cHash=12028cb059
[2]: http://blog.wearesakuzaku.com/making-netatalk-work-on-debian-with-leopard/
[3]: http://www.damontimm.com/blog/how-to-install-netatalk-afp-on-ubuntu-with-encrypted-authentication/
