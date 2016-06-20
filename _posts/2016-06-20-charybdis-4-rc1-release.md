---
layout: post
title:  "New directions for Charybdis, starting with Charybdis 4"
---

We are pleased to announce both the immediate availability of [charybdis 4 release candidate 1][rc1], as well as an exciting new 
directions for the charybdis project in general!  When charybdis was started at atheme.org, the purpose of the project was originally
to provide a working IRCd that was specifically designed to leverage the new features introduced by atheme.org both in Atheme as well
as the later IRCv3 incubation that occured there.  As part of atheme.org's restructuring, the IRCv3 and charybdis projects graduated
into their own autonomous projects, which has resulted in many people asking what the future of charybdis was to be.  The answer, simply
put, is that charybdis 4 should provide a preview at what we feel the future of the project will be.

   [rc1]: http://distfiles.charybdis.io/charybdis-4-rc1.tar.bz2

## What's new in charybdis 4?

Here's a general overview of what we've done in charybdis 4.  We will be following up these changes with charybdis 5 later this year.

### authd: a new authentication engine for charybdis

Right now, the core IRCd provides a lot of functionality that may crash the IRCd.  In charybdis 4, we have built an entire new
authentication engine which exists outside of the IRCd process called `authd`.  You may have heard about other work in this area,
such as `iauth`, but `authd` is a complete from scratch reimagining of the concept.  The next step for `authd` is to handle D/K-lines,
and actually authorize access to the IRCd in general.  This will allow us to more easily leverage it's modular design to allow various
types of authentication with external services in the future.

Best yet, this work is integrated directly into `ircd.conf`, so you do not need to configure many different daemons as you do with `iauth`.
It just works, and it uses the settings you're already using.  On top of this, `authd` can also do proxy scanning at connect time, further
hardening your network against bot floods.  The new, optional, proxy scanning functionality is just the beginning of what we plan to do
with `authd`.

### wsockd: a new WebSocket engine for charybdis

We have also added experimental support for WebSocket to the IRCd, via a new translator daemon called `wsockd`.  This support automatically
stacks with `ssld`.  `ssld` can of course multiplex normal socket connections and websocket connections on the same set of daemons, for maximum
efficiency.  As far as we know, charybdis 4 is the first IRC server with native websocket support.

The inclusion of WebSocket support will allow IRC networks to leverage the capabilities of modern web browsers.  With a web IRC client that supports
WebSocket, users accessing IRC from their browser no longer need to worry about being second-class citizens.

### improved IRCv3.2 support

charybdis 4 features support for almost all of the IRCv3.2 extensions, with the only notable exception being `batch`.  We are going to have
to make some changes to the way netbursts and netsplits are handled to properly support `batch`, and we also have some ideas on how to improve
`batch` semantics which we will be working with the IRCv3 group to implement in IRCv3.3.

### changelog

There have been many more changes in charybdis 4, we encourage you to read the [NEWS](https://github.com/charybdis-ircd/charybdis/blob/release/4/NEWS.md)
file to find out about all of the changes!

## Future directions of charybdis

Now for the exciting part where we talk about what comes next!

### spamfilter

We are working on bringing a high-performance spamfilter implementation to charybdis 5.  It is modular and can be enabled/disabled on a per-channel basis,
giving channel operators the ability to control whether or not the feature is used on their channels.  We have both a PCRE2-based regex filter as well as
a filter which mitigates nickname highlight spam attacks.  This will be integrated into the `master` git branch soon.

### Advanced Operator ACL

In addition to spamfilter, we are bringing an advanced operator ACL system to charybdis.  Almost all aspects of the network can be configured, including
enhancements to the server protocol to provide network-wide knowledge of a user's capabilities.  In typical charybdis fashion, this system will be
accessible and easy to use.

### IRCv3

The charybdis team will continue to collaborate with the IRCv3 group on new additions to the client protocol.  We don't have anything we're working on
in specific here, but we plan to formally begin participation in the IRCv3 group shortly.

### Dynamic `CHANTYPES`

We also plan to add configurable channel prefixes to the IRCd.  These allow you to configure different channel types with different distribution and behaviour
settings... which leads us to...

### Federation

Finally, as a research project we are working on adding native support for federation to the IRCd.  The first parts of this will land in charybdis 5, and allow
networks to link together by having the peer network separated into it's own namespace.  This will be supported by the dynamic `CHANTYPES` API.

The eventual goal?  A universal metanetwork that is enabled by participant networks peering with other networks, and universal namespaces.  We plan to bring this
by the charybdis 7 or 8 lifecycle.  Beyond this, we plan on heavily documenting the federation protocol so that other daemons may participate in the metanetwork.
