---
layout: page
title: Mining
wikiPageName: Mining
menu: wiki
---

## Introduction
Mining in Purple, while being superficially similar to mining on any other Proof of Work blockchain, has a completely different process which has more steps than the classic Proof of Work model. While mining is directly tied to approving transactions in classic blockchains, this is not the case in Purple.

Instead, mining is used **to determine the next validator set** that is to be added to the validator pool. That is the only purpose of it. Successful miners have to participate in the validator pool and to approve transactions in their designated lifetime in the pool before collecting any rewards.

There are also two chains which can potentially be mined, which for simplicity, have been named the `HardChain` and the `EasyChain`. The `HardChain` has a well, higher difficulty and coinbase rewards while the `EasyChain` has a lower difficulty but **no coinbase rewards**. Each block in either the `EasyChain` or the `HardChain` will state the identity of **exactly one validator** that is to be joined into the pool.

### Epochs
Each block that is mined on the `HardChain` represents a consensus **epoch**. For each epoch, the pool has a designated amount of blocks that it can produce while at the same time, each validator has a designated amount of events that it can produce (events are contained by blocks). When the designated blocks have been consumed by the pool, the next `validator set` is joined into the pool while previous validators that do not have any allocated events remaining are retired and they collect their rewards.
