---
layout: post
title:  "Purple Protocol April 2019 Development Update"
date:   2019-05-07 13:24:41 +0300
author: Octavian Oncescu
categories: purple development update
---

For the past month, we have been mostly focusing on the second part of the consensus logic: implementing the code responsible for storing and choosing chains of blocks. Along the way, we have also made a few simple but effective architecture updates which smooth out the state transitions of the ledger and thus make it more secure.

### Background
A few months ago, I have received feedback on the whitepaper from an unknown computer scientist, only known to me as "Dr. Brain". He said that he was enthusiastic about the solution that was presented in the whitepaper and said that it has the chance of actually solving the scalability problem. But at the same time, he noticed two big pain-points:

* Even though the shuffled validator pool idea makes sense, it is very hard to actually join validators to the pool in a secure way. In simple words, what happens when a validator joins during the execution of the asynchronous algorithm? This state transition, if not done properly, can have catastrophic results.

* If the scalability problem is solved, this leads us to another quite exotic and intrinsic problem: When megabytes of transactions will continuously written to the ledger each second, the size of the ledger will grow **very** quickly with even moderate use.

In this development update, we will present how we have solved the first pain-point presented by Dr. Brain. 

### Architecture updates
In order to not break the validator pool, we have to make the state transitions explicit somehow. To achieve this, we will need to determine the sequence in which validators are joined when put against the stream of blocks that is produced by the pool. 

#### Joining and retiring validators
Since the problem lies in joining validators at the same time as the pool produces blocks, we will have to separate these two things. The solution for this is the following:

* For each block mined in the validator chain (the chain responsible for storing blocks which mark validators as being eligible to enter the validator pool), the whole pool is assigned a fixed number of blocks that it can produce with that specific validator set.

* Once the pool consumes all of its allocated blocks, it can safely add or retire validators i.e. apply the next block from the validator chain to the pool state. 

* This means that each individual validator, as well as the pool as a whole have an allocated number of blocks that they can produce, which denotes their lifetime in the consensus process.

* This also means that each validator chain block maps to one synchronous step in the consensus algorithm.

* If a validator misbehaves and proof of this is found, the validator pool will immediately skip to the next state transition i.e. it will remove the bad node and join new nodes.

#### Joining more than one validator at each step
The solution is great but there is still one problem left: how do we keep the pool at an optimal level of validators (not too few) **from the beginning**? The ideal is that the validator pool will **always** have an optimal number of validators. But in the beginning, with the presented model we cannot have but one validator.

This is a problem which might endanger the growth of the network, especially in its early stages, and can make it susceptible to attacks.

The solution for this is to join more than one validator in one step, thus removing any chance of the pool having too few validators to operate safely. 

#### A trinity of chains
The solution is to split the validator chain into two separate chains. The first of them has mostly the same role as the previous validator chain: mark a synchronous step in the consensus algorithm and to introduce one validator to the pool (the miner of the block). 

The second chain will be responsible for **buffering additional validators** which will be also added to validator pool. This chain will also have lower difficulty than the previous one but will yield no coinbase rewards, but successful miners of this chain who become validators will still receive mining fees as reward at the end of their consensus lifetime.

For simplicity we shall name these two chains the **Easy Chain** and the **Hard Chain**. The hard chain is the chain with the higher difficulty which yields coinbase rewards and the easy chain has a lower difficulty but has no coinbase rewards, the miners will just receive transaction fees.

The result is that we can adjust the minimum requirement of validators required before the pool can operate with this model while at the same time offering a more democratic entry opportunity for potential validators.

The hard chain, along with the easy chain and the state chain form a "trinity" of chains that together store all of the state transitions in the ledger state.

### Test-net status
What has been presented so far is almost implemented in the [source code](https://github.com/purpleprotocol/purple/pull/34) and is now in the debugging and testing phase. The code should be merged in just a few weeks. After this, we only have to test the chain and consensus modules together, write the network layer, and tie them together.

I estimate all of this will be handled in the next 2 to 3 months, maybe less if we don't encounter any roadblocks along the way.

### Summary
This month's development update has been a bit more comprehensive than that of the [last month](https://purpleprotocol.org/blog/end-of-march-development-update) and has also been written a little later. However, the news it brings are good and the future seems to be looking bright.

With each passing day we are closer the the anticipated launch and this makes me feel really enthusiastic.
