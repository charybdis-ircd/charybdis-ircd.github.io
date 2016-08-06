---
layout: page
nav_title: Download
title: Download charybdis
---

## Tarballs

The current stable release series is 3.5.

  * [charybdis 3.5.2 source](http://distfiles.charybdis.io/charybdis-3.5.2.tar.bz2)

The latest testing release series is 4.

 * [charybdis 4-rc3 source](http://distfiles.charybdis.io/charybdis-4-rc3.tar.bz2)

Older versions are available at [distfiles.charybdis.io](http://distfiles.charybdis.io).

## Latest sources from GitHub

The charybdis ircd sources are maintained on GitHub.  To clone the repository, run these commands:

{% highlight bash %}
$ git clone https://github.com/charybdis-ircd/charybdis
$ cd charybdis
{% endhighlight %}

This will check out the *master* branch by default, which has experimental code that may or may not be
suitable for production networks.  We recommend checking out the stable release series branch, which
at present is 3.5:

{% highlight bash %}
$ git checkout release/3.5
{% endhighlight %}

You can also view our source repository on [GitHub](https://github.com/charybdis-ircd/charybdis).

## Building

To build charybdis, run the following commands from your charybdis source directory:

{% highlight bash %}
$ ./configure
$ make
$ make install
{% endhighlight %}

By default, the charybdis binary distribution will be installed into `$HOME/ircd`.  We recommend using
this deployment method.

## charybdis in Linux/Unix Distributions

Some distributions offer charybdis as a package.  These distributions may or may not be shipping the latest
version, so if you experience problems we recommend building charybdis from source using the steps listed
above.  Many bugs are fixed in each new release.
