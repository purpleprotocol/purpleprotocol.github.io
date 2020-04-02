---
layout: post
title:  "Purple Protocol May 2019 Development Update"
date:   2019-05-31 13:24:41 +0300
author: Octavian Once
categories: purple development update
description: This month's development update is all about mining. Or rather, the mining algorithm behind Purple...
--- 

This month's development update is all about mining. Or rather, the mining algorithm behind Purple: `Cuckoo Cycle`. By using this algorithm instead of the traditional `HashCash` which is used by Bitcoin, the mining will be totally **ASIC resistant**. At least in the beginning.

### What are ASICs?
Some of you who are still new to the cryptocurrency space and are reading this might be wondering what are ASICs. The short answer is that they are the **enemy of decentralization** in Proof of Work blockchains.

What they are in actuality is best described by the following: **ASICs are custom hardware that is built with the purpose of solving one specific problem as effectively as possible**. For example, there are ASICs that solve Bitcoin's HashCash **thousands of times** faster than any GPU.

When ASICs roam in the wild, they compromise the decentralized nature of a blockchain network by allowing only a few hardware producers and select customers to benefit from mining on that network. This clearly makes a democratic selection of miners impossible.

### Cuckoo Cycle
Purple will be using the **cuckaroo** variant of the Cuckoo Cycle Proof of Work algorithm. It was invented by [John Tromp](https://github.com/tromp) with the intent to provide a memory-hard Proof of Work which has ASIC resistance. 

We have also established communication with John who has agreed to advise the project on how to best use Cuckoo Cycle.

#### How does Cuckoo Cycle provide ASIC resistance?
In HashCash, the difficulty of the puzzle is directly dependant on the processing power of the miner. The higher the processing power of the miner, the faster it will solve the puzzle.

In Cuckoo Cycle, processing power does not help you at all. This is because it relies on the bottleneck of **memory bandwidth** instead of a cpu bottleneck.

In simple terms, there is so much memory that you can read in a limited amount of time. The cpu has to wait for memory to be fetched from the RAM before it can check it, so having a fast processor is literally useless. 

Cuckoo Cycle leverages this property to provide an ASIC resistant memory-hard proof of work. If you are interested to read more about Cuckoo Cycle, the best place to do so is the [whitepaper written by John](https://eprint.iacr.org/2014/059.pdf) or the [explanatory blog post](http://cryptorials.io/beyond-hashcash-proof-work-theres-mining-hashing/).

#### Long-term ASIC resistance
Since the development of ASICs is always inevitable, after launch, Purple will perform hard-forks to change the mining algorithm, similarly to [how grin does it](https://www.coindesk.com/grin-cryptocurrency-to-vote-on-change-to-hard-fork-roadmap).

We do not yet plan to do this on a schedule since it might be useless to do it in the first place. It all depends on the development of novel ASICs. For now there are no known ASICs for the cuckaroo variant of Cuckoo Cycle, however its other variant cuckatoo, already has a working ASIC.

### Test-net status
While most of the important code has been written, which includes most of the virtual machine, consensus logic and transaction logic, we still have to write the following things:

* The network layer and runtime which ties everything together.
* The mempool.
* The RPC interface.
* The terminal UI.

It is hard to estimate how long this will take. We can possibly reach a **very basic** testnet in 1-1.5 months. But this can extend further, since we don't know what roadblocks might be encountered along the way. 

But so far, the sea seems to be calm and the skies clear.

### Summary
This month's development update was all about the Cuckoo Cycle algorithm and how it will be used as the mining algorithm behind the Purple Protocol in order to make it ASIC resistant.

We have also established a communication channel with Cuckoo Cycle's inventor: John Tromp. John has agreed to advise us on how to best use his invention.

If you like what you have read here, help us spread the word about Purple by sharing this blog post with whomever you believe might be interested to hear about this. Also, if you want to help us continue or journey or if you want to join it, you can do so by reading our [Contribution Guideline](https://github.com/purpleprotocol/wiki/wiki/Contribution-Guideline) or by [donating funds](https://purpleprotocol.org/blog/participate-in-purple-crowdfunding-campaign) to our cause.

We thank you if you wish to contribute to the development of the Purple Protocol and we are excited for the almost near launch of the first test-net. Stay tuned!

