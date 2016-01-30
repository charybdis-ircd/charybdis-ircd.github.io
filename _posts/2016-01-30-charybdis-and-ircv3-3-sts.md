---
layout: post
title:  "charybdis 3.6, IRCv3.3 STS, the generally oncoming IRC TLS transition and how to manage it"
---

The IRCv3 working group have committed to transitioning IRC from plaintext to TLS with the 3.3 release.
They plan to do this using an extension called ["IRCv3 Strict Transport Security"](http://ircv3.net/specs/core/sts-3.3.html), which will
use the capability negotiation mechanism to notify clients to switch to TLS mode.  In general, we agree
with the goal of transitioning IRC to TLS in 2016, but we question whether STS is a sufficient transition
mechanism, as well as [whether or not it should be included in core or instead as an extension](https://github.com/ircv3/ircv3-specifications/issues/207).
This post will describe an alternative transition plan which encourages TLS usage by inverting the functionality
of the secure-only (`+S`) channel mode, and should serve as the recommended transition plan for operators of
charybdis networks.  The eventual TLS transition has been discussed with the charybdis network operator community
at large for several years.

### Operating charybdis 3.6 in transition mode

Migration of an entire network from plaintext to TLS has a large social impact.  Users are required to reconfigure
their clients and/or upgrade to a client which supports a sufficiently secure TLS version.  We agree that "baby steps"
are required to facilitate a transition.

To facilitate this, the charybdis 3.6 tree has a few extensions that come together to facilitate a transition
process.  These are:

 * `extensions/chm_insecure.la`
 * `extensions/tlswarning.la`
 * `extensions/createtlsonly.la`

#### `extensions/chm_insecure.la`: the opposite of secure-only

This is the evil twin of `extensions/chm_sslonly.la`.  It provides the functional opposite of the `+S` channel mode
as `+U`: instead of *requiring* TLS to join the channel, it instead *permits* non-TLS users to join the channel.  The
net effect is that the status of the channel being *insecure* is now displayed instead of the channel being *secure*.

What this means is that users are now aware of their chats being insecure, and should be encouraged to take action to
secure them.

#### `extensions/tlswarning.la`: warn non-TLS users that they should use TLS

The TLS warning extension allows the network to warn non-TLS users on a regular basis that they should upgrade their
connections.  It advises them that the non-TLS service is deprecated and will be eventually shut off.

#### `extensions/createtlsonly.la`: restrict channel-creation events to TLS users

This module restricts channel creation to TLS users, which makes sense if you're not putting `+U` in `default_cmodes`.

### Bringing it all together

A basic transition plan for a network is as follows:

1. Load `extensions/chm_insecure.la` and `extensions/createtlsonly.la` on your network.
2. Mass-set `+U` on all pre-existing channels that are not `+S` (use Atheme or Anope to accomplish this easily)
3. Remove `+S` on all pre-existing channels (its not needed anymore as `-U` functionality is equivalent)
4. Explain to users what `+U` means, and why they do not want it.  At this point, mention when plaintext support will
   be discontinued.
5. Secure your WEBIRC gateway, if any.  An extension to the WEBIRC command will land soon to allow the IRCd to know
   if the gateway connection is actually secure.
6. Allow time for users to migrate to TLS connections.
7. Once users have migrated, shut down the plaintext ports.

If you have any questions, feel free to drop by `#charybdis` on freenode.
