---
layout: post
title:  "authd: a new authentication system for charybdis"
---

Since the inception of ircd (the late 80's), ident and DNS lookups have been done in the ircd process. In almost every ircd 2.7 (ircu/snircd) or 2.8 (hybrid/ratbox/charybdis/dreamforge/unreal/bahamut/etc.) derivative, they are still done in the main ircd (though ratbox moved DNS to its own daemon). Obviously they've all undergone iterations over the years (such as removal of the DNS cache from hybrid before the ratbox fork, and the way idents are checked has been tweaked in most IRC daemons), but overall the architecture is essentially unchanged.

The reasoning behind doing these lookups in-process is that DNS and ident lookups were only done once per connection (in what I will call the ***pre-registration process***, performed by the ***pre-registration mechanism***, which has to do with clients "registering" with the server, not services), and therefore should only use negligible resources compared to everything else going on. While it's still true ident lookups are still only done once per connection, in Charybdis, DNS connections are not. In Charybdis, a DNS lookup is done for every blacklist configured in the IRC daemon once per connecting client (usually resulting in about 3 lookups that have to be processed). Other IRC daemons often use a file descriptor per lookup (though Charybdis does not).

It must be admitted that the scenario of running out of resources has not been witnessed in practise. Client connection overhead is also still very low; DNS (and blacklist) lookups are generally very cheap, and ident lookups cost almost nothing. However, if we were to introduce, say, open proxy checking, the number of file descriptors per connecting client would certainly skyrocket. Running out of file descriptors is a very real possibility if you are scanning dozens of ports at once per connecting client (it is ideal to do it before the client registers). It could also tie up the event system with processing lots of connections to ports probing for proxies.

There is also another problem: the present pre-registration mechanism for Charybdis is relatively inflexible and difficult to make changes to. It was simply never designed to do more than look up ident and rDNS (bans are checked after clients finish connecting). Blacklists, for instance, were basically implemented as hacks *around* the old ident and rDNS checking code. A lot of fragile state had to be kept by the old authentication code whilst it did DNS and ident lookups. In the present state of the code, stuff like open proxy scans are difficult to implement, and risk consuming too many file descriptors. However, rather than *only* offload blacklist and any other future queries to their own daemon, it's better for the entire pre-registration process to stay self-contained.

Enter authd
===========
We have decided to move all of these checks to a new daemon called **authd**. It does DNS lookups (including rDNS), ident queries, and blacklist scans. Right now authd is pretty simple; it just moves the pre-registration process to its own daemon. However, it has been written in a way where it's easy to write **providers**, which add additional functionality. A prototype open proxy scanning module is in the works as we speak. Other authentication providers are possible, such as two-factor authentication to servers.

Adminstrators and opers
=======================

Administrators generally don't have to do anything; authd is always enabled, working, and obeys all your usual configuration options. Everything should be business as usual, though some stat output related to blacklists has changed in minor ways.

Users
=====

Users may notice slightly snappier registration, and slightly different notices before connection. If ident is disabled, we now inform the user of this fact.
