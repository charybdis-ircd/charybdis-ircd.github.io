---
layout: page
nav_title: Download
title: Download charybdis
---

## Tarballs

The current stable release series are 3.5 and 4.

 * [charybdis 3.5.5 source](https://github.com/charybdis-ircd/charybdis/archive/charybdis-3.5.5.tar.gz)
 * [charybdis 4.0 source](https://github.com/charybdis-ircd/charybdis/archive/charybdis-4.0.tar.gz)

## Latest sources from GitHub

The charybdis ircd sources are maintained on GitHub.  To clone the repository, run these commands:

{% highlight bash %}
$ git clone https://github.com/charybdis-ircd/charybdis
$ cd charybdis
{% endhighlight %}

This will check out the *release/4* branch by default. If you want to use version 3.5, check it out:

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

Note that version 4 requires you to first run autogen.sh

By default, the charybdis binary distribution will be installed into `$HOME/ircd`.  We recommend using
this deployment method.

## charybdis in Linux/Unix Distributions

Some distributions offer charybdis as a package.  These distributions may or may not be shipping the latest
version, so if you experience problems we recommend building charybdis from source using the steps listed
above.  Many bugs are fixed in each new release.
