---
layout: post
title:  "Purple Protocol Late January 2020 Development Update"
date: 2020-02-01 13:24:41 +0300
author: Octavian Oncescu
categories: purple development update
description: This month's development is all about the steps left to take before we can have a test-net up and running...
--- 

This month's development is all about the steps left to take before we can have a test-net up and running. Purple has now almost been a two year journey and even though we encountered roadblocks along the way, we are in the final stage of the development phase and will soon enter the testing phase.

### Development status
With the core consensus modules being finished and most of the state modules and protocol logic in place, we are now focusing heavily on developing the network layer of the protocol. 

There are main things stopping us from running a network right now:

#### Block propagation 
Implementing the [Graphene Protocol](https://medium.com/coinmonks/graphene-block-propagation-protocol-aceb8730fa5e) as the block propagation mechanism. This allows us to propagate less block data which results in a larger transaction throughput. Compared to a Bitcoin block of 1mb, these blocks would only be ~20kb in size while storing the same number of transactions.

While these improvements are incredible, the Graphene Protocol is definitely more complex than the simple propagation method used in Bitcoin. We are now actively implementing Graphene on top of the Purple Protocol.

#### Chain sync
While Graphene optimizes block propagation for nodes that **are in sync**, there is still a big problem that remains. This is especially true in the case of Purple which aims to have a throughput of at least several thousand transactions per second. 

When nodes first join the network, they have to download the chain-state which can be 10s of gigabytes in order to be able to validate future blocks. One way to handle this issue is to simply reduce the size of the state. **Purple will use 8 byte trie keys instead of 32 bytes**. For 1 billion keys this means **the difference between storing 32gb of data and 8gb of data** while achieving the same end result.

#### Server and development costs
Another big issue we have right now is the lack of resources and funding. While some people have contributed financially by [donating](https://purpleprotocol.org/blog/participate-in-purple-crowdfunding-campaign), we are still pretty much self-funded at this point. As we approach the point where we can run a network we are going to need funding in order to sustain the test network.

### The way forward
Even though we are having difficulties, which is normal, we are still progressing at a fast pace. Most of the consensus-related code is written by this point. Transaction propagation works and soon, block propagation will work as well. We hope that we will be able to solve the remaining problems soon and then launch the Phase 1 test-net.

We have to thank the community for being patient and for supporting us through this journey.

### Join Us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread The Word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!