---
layout: page
title: Block Propagation
wikiPageName: Block-Propagation
menu: wiki
---

This document describes the block propagation mechanism in the Purple Protocol. Block propagation in the Purple Protocol is done using the [Graphene Protocol](http://cryptoeconomics.cs.umass.edu/graphene/graphene-short.pdf).

### Table of Contents
1. [Introduction](#introduction)
2. [Block propagation in Bitcoin](#bitcoin-block-propagation)
3. [Graphene Protocol](#graphene)

### Reasoning <a name="introduction"></a>
One of the fundamental scalability bottlenecks in decentralized blockchains is the **block propagation time**. A simple way to explain block this is the time it takes for a block to propagate to all of the peers participating to the network.

When a peer receives a block, it must validate it and decide if it will propagate the block further by sending it to its neighboring peers. 

### Block propagation in Bitcoin <a name="bitcoin-block-propagation"></a>
Bitcoin uses the simplest form of block propagation that exists. When a node creates or sends a block, it will take the header of the block along with the transaction data and send it to all of its peers. The problem with this is that most of the transactions in the block are already known by the receiving peer. 

While this approach is crude, it does yield results and the stability of the Bitcoin protocol over the years prove it. However, this approach does not satisfy the needs of a scalable system. It takes 8 hops for a packet sent in a network via flooding for the probability of most of the nodes in the network to receive a packet to be 100%. 

Now, this means that in order for a bitcoin block to fully propagate to the network it takes at least 8 hops. If a block has a size of 1mb, the propagation time of that block will be the average amount of time it takes to transfer 1mb of data via a TCP connection in a low-bandwidth scenario **times 8**. This is without taking into account the amount of time it takes to validate incoming transactions against data stored on disk.

The [stats](http://bitcoinstats.com/network/propagation/) say that a block takes an average of 6 seconds to propagate. And this makes sense. In order to achieve rapid finality, we must reduce this number to potentially hundreds of milliseconds.

### Graphene Protocol <a name="graphene"></a>
The Graphene Protocol was proposed as a way to address these problems in Bitcoin. It is based around the idea that most transaction data found in blocks is **mostly already known by receiving peers**. This means that we do not have to send all of the transaction data in a block if we know which transactions are already known by the receiver.

Graphene uses Bloom Filters paired with [IBLTs](https://arxiv.org/abs/1101.2245) to achieve exactly this. Instead of simply sending all of the transaction data along with the block header, a node will first send just the header along with a bloom filter and a IBLT containing all the transactions in the block. The receiving node will then match its locally known transactions against the bloom filter and assemble the block locally.

If the receiving node does not have all of the transactions in the block, it will identify the missing ones using the IBLT and query other nodes for the missing transactions.

This reduces block propagation time exponentially when transactions are already known to the network. In case of miner created transactions, it will fallback to the default propagation time found in Bitcoin.
