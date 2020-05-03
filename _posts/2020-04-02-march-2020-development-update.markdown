---
layout: post
title:  "Purple Protocol March 2020 Development Update"
date: 2020-04-02 00:24:41 +0000
author: Octavian Once
categories: purple development update
description: In this month's development update we are going to talk about the latest design changes we have made in the network layer...
--- 

In this month's development update we are going to talk about the latest design changes we have made in the network layer in order for it to be as efficient and secure as possible. 

In particular, we are going to talk about [QUIC](https://en.wikipedia.org/wiki/QUIC) and why we have chosen it as the default transport protocol instead of TCP.

### Why QUIC?
QUIC stands for "Quick UDP Internet Connections" and was designed by Google as a better alternative over TCP/TLS. It has been used in production at Google since at least 2013 with impressive results.

The main benefits of QUIC over TCP are as follows:
* Multiplexed by default. We don't have to worry about [Head-of-Line Blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking).
* Runs over UDP which is always faster than TCP.
* Encryption is provided by default.
* Optimised for wireless connections.
* Prevents IP spoofing.

This means that just by switching to QUIC, we increase both the network throughput and its security.

However, as with all good things, there is always at least one downside. In the case of QUIC it is that some ISPs throttle UDP traffic. This results in an exponentially lower performance than simply using TCP.

### TCP as fallback
While ISPs throttling UDP is uncommon, in the few cases it does happen, the user can fallback to using TCP by passing the `--tcp-only` flag when running the core. 

When using plain TCP, we will be handling encryption and multiplexing ourselves in the application space. 

### Implementation status and COVID-19
We are currently implementing these changes in the core protocol. This has been our main activity since around the beginning of the COVID-19 pandemic in Europe. 

I am now confined alone in my flat in London and all I can do is further develop the core protocol. I have enough tea stocked up to keep an elephant awake for a whole week. The other core developers are also healthy and confined to their bedrooms.

We are lucky to be able to continue the development during the pandemic as many other people have been alienated from their daily activities. 

As always, we will carry on regardless of what life throws at us.

### Join us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread the word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!