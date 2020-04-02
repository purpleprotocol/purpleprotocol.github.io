---
layout: post
title:  "Purple Protocol February 2020 Development Update"
date: 2020-03-02 13:24:41 +0300
author: Octavian Once
categories: purple development update
description: This month's development is about block propagation and how we are solving this in Purple...
--- 

This month's development is about block propagation and how we are solving this in Purple. The block propagation rate is one of the most important things to consider when thinking about scalability in blockchain systems. Without a high block propagation rate, we cannot achieve the fundamental breakthrough that we are aiming for. 

### Background
Block propagation mechanisms have much evolved since the Bitcoin protocol was first conceived. Some of these advances in this have also been applied to the Bitcoin protocol itself. 

Both [Compact Blocks](https://bitcoincore.org/en/2016/06/07/compact-blocks-faq/) and [Graphene](https://people.cs.umass.edu/~gbiss/graphene.pdf) schemes focus on transmitting less block data on each network hop. However, the cost of achieving this comes as increased round-trips. 

They also both suffer the same fundamental flaw that was also present in the original propagation system of Bitcoin: on each hop the whole block has to be downloaded and validated before being propagated further.

What we want is to minimize round-trips as the average round-trip time is around 200ms. We also want nodes to be able to download a single block from multiple, vetted sources in parallel.

### Bittorrent
The problem of block propagation can be thought of as a file replication problem with a few caveats. We can look at how the Bittorrent protocol achieves this and how it is optimized for maximizing bandwidth.

There are a few key concepts that make the Bittorrent protocol achieve optimal file replication:
* Each file is broken into chunks (or pieces) of usually `256kb` in size.
* The torrent file contains the hash checksum of each piece.
* Each piece is further divided into sub-pieces, of usually `16kb` in size.
* A requesting peer selects pieces and downloads them in parallel from multiple peers.
* Each requesting peer selects a few best peers to download and upload from based on previous download/upload rate and availability.

### Purple Parallel Propagation
If we think of a block as a torrent, we can then begin to draw inspiration from Bittorrent and achieve optimal block propagation:
* Transaction data can be split as in Bittorrent and a transaction block header will contain `8byte` checksum hashes of each chunk.
* Block headers are propagated as previously but without waiting to validate the transaction set.
* Nodes will then begin requesting pieces of the block from its peer set, prioritising pieces with a lower height.
* At a `4mb` block size we only need to store `128 bytes` of checksums in the header.
* A node will upload pieces of a block to other peers without waiting for the whole block to download.

By using this mechanism, the peers with the best download/upload speed will mostly always be able to stay in sync while nodes with a lower internet speed will not slow down the faster nodes.

### Implementation status
While this change will definitely delay the release of Purple, we have currently began implementation of the parallel propagation mechanism. While we don't know yet any numbers, we expect an exponentially higher transaction throughput after the implementation is complete.

### Summary
In this development update we have presented the final block propagation mechanism that will be used by Purple which was inspired by the way Bittorrent handles file replication on a peer-to-peer network.  

### Join Us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread The Word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!