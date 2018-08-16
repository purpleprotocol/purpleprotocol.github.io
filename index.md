---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
permalink: /
title: null
---

## What is Purple?
Purple is a platform for building highly scalable decentralized applications. Using a novel consensus algorithm and by employing sharding, Purple can reach a throughput of around 1.2 million transactions per second, making it the fastest decentralized protocol that exists. 

## How is Purple different from other platforms?
Purple uses neither Proof of Work nor Proof of Stake in order to achieve consensus. A number of nodes called sequencers are responsible for processing transactions. 

Sequencer nodes then achieve consensus by employing a fast, asynchronous consensus algorithm which sacrifices a bit of resilience in favor of speed. 

Since the actual consensus algorithm is resillient as long as a third or less of the nodes implicated in it are malicious, the idea is to use Proof of Work to rate-limit the number of nodes participating in the consensus algorithm while randomizing the chance of becoming one, making participation highly decentralized. 

A node becomes a sequencer by providing a valid Proof of Work on the last state of active sequencer nodes and the difficulty of the algorithm is adjusted dinamically to always maintain a set of active nodes that are owned by different entities.

By using Proof of Work in this way, the security of the network is similar to a full-fledged Proof of Work protocol but without sacrificing speed.

## What similarities does Purple have with Ethereum?
Purple seeks to improve upon pioneering work of Ethereum, which was the first decentralized applications platform. While being truly revolutionary when it was released, it also suffered from several flaws which were really hard to avoid at that time, the biggest of which is it's poor scalability. 

For this reason, Purple shares many design decisions that were proven successful with the Ethereum protocol while improving on the parts that were not so great. 