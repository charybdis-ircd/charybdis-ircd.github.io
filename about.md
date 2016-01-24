---
layout: page
nav_title: About
title: About charybdis
---

charybdis is a highly scalable IRC server which presently implements the IRCv3.1 client protocol with some additional components of IRCv3.2.
Development of charybdis began in 2005 as a proposed replacement to freenode's hyperion ircd, designed for use by any network, with roots
in ircd-ratbox and ircu.  Over time, charybdis has developed, tested and demonstrated many of the features commonly seen in modern IRC networks.

Interesting features that charybdis has include:

  * SASL support &mdash; charybdis was the first IRC server to implement SASL support, resulting in the IRCv3 SASL standards.
  * DNS blocklist support
  * Augmented banlist criteria through "extbans"
  * Channel forwarding (channel mode +f)
  * Optional hostname/IP cloaking (through modules, either usermode +h or +x)
  * Colorcode stripping (channel mode +c)
  * CALLERID (user mode +g) with automatic accept when you send a PM
  * TLS encrypted client and server connections
  * Multi-process sandbox architecture for enhanced security, scalability and robustness
  * Easy to understand configuration
  * [A complete user and operator's manual](http://www.stack.nl/~jilles/irc/charybdis-oper-guide/index.html)

Many of the largest networks in the world use charybdis-based IRC servers, including freenode, EsperNet, DarkMyst and others, because of it's
proven scalability, security and robustness track records.
