---
layout: post
title:  "Purple Protocol November 2019 Development Update"
date: 2019-12-01 13:24:41 +0300
author: Octavian Oncescu
categories: purple development update
description: This month's development update brings several changes and simplifications to the core consensus architecture of the protocol...
--- 

This month's development update brings several changes and simplifications to the core consensus architecture of the protocol. 

### Background
In the past two months, I have been trying my best to launch an alpha version of the Purple Protocol by tying up together all of the existing components. 

As I was starting to test the code on more and more nodes, it became apparent to me that the current architecture worked with a single exception: the current model assumes that the ledger state is separate from the proof of work state. It also means that the ledger state would be entirely dependent on the proof of work state (i.e. which validators are currently active and allowed to produce blocks). The problem becomes when the proof of work state has to depend on the ledger state. 

If we want the proof of work to secure the ledger state as is in the case of bitcoin, we would have a circular dependency like this: `PowChain <- StateChain <- PowChain`.

This is an almost impossible scenario. While cyclic dependencies can sometimes be handled using special logic, it is *always* prone to dubious bugs.

The other possible scenario would be that we do not require the Proof of Work to secure the ledger state, but this means that the ledger state effectively becomes malleable which to me seems like a great source of attack vectors.

As you can see, this problem which, because I was so immersed in the development of it was not apparent to me until now, kind of defeats the whole architecture of the protocol.

But, as is with all great experiments, sometimes a great failure paves the way forward for a great success...

### Solution
As the old model quickly faded away in my mind, a much simpler model emerged. As I was thinking of removing cyclic state dependencies it became apparent to me that the solution is to remove the components themselves which naturally create a cyclic state dependency. 

The solution is to store both the miner information and transaction data on the same chain as is the case of bitcoin. This consensus change requires compiling a new white-paper which has not yet been done due to time constraints on my part. However, the next sections of this article will detail the key concepts of the new consensus model until an adequate white-paper will be compiled.

The source code for the new model can be found on the official repository under the [consensus_v2 branch](https://github.com/purpleprotocol/purple/tree/consensus_v2).

#### Purple Consensus V2
This much simpler consensus model has much less moving parts and at this point is only dubbed as `PurpleConsensusV2`. It applies the same concept of choosing a validator entity via Proof of Work who is entitled to create a number of blocks of transactions that are to be applied to the ledger state.

#### Single validator instead of a validator pool
By limiting the consensus rules to choosing only a single validator on each step, we remove the need for any synchronization logic needed between active validators.  

The rule then becomes simple: A successful miner is entitled to create `k` subsequent blocks that form a chain that are children of the initial mined block which contain sets of transactions. `k` is either a constant or dynamic protocol parameter. These child blocks do not require any proof of work but are limited by the parameter `k`.

#### Checkpoint and transaction blocks
A `CheckpointBlock` must contain a valid proof of work on the whole chain at the moment in time where it is created. A `CheckpointBlock` does not contain any transactions and is used only to grant eligibility to the miner to create transaction blocks.

A `TransactionBlock` can only be created by a miner which has successfully announced a `CheckpointBlock` with a valid proof of work and can only be the child of such a block or another `TransactionBlock`.

A checkpoint block can only be the child of either the genesis block or a final transaction block (which has the height equal to that of the height of the initial checkpoint block summed with `k`).

To illustrate these rules, let's assume that we have a network of 2 nodes `A` and `B` and a fresh chain with only the genesis block. Now, we have parameter `k` equal to 3 which means that a miner must append 3 transaction blocks after a checkpoint block. If node `A` finds a valid proof of work and fulfills the condition, the chain will look like this:

```
Genesis -> CheckpointBlockA1 -> TxBlockA1 -> TxBlockA2 -> TxBlockA3
```

Miner `B` has a choice: he can either create a checkpoint block that follows the `Genesis` block, thus creating a fork, or he can create a checkpoint block that follows `TxBlockA3` and builds upon the chain created by miner `A`: 

```
Genesis -> CheckpointBlockA1 -> ... -> TxBlockA3 -> CheckpointBlockB1 
-> ... -> TxBlockB3
```

The same rule applies where the longest chain is always chosen by all nodes.

#### Stale validators
Since creating a `CheckpointBlock` requires following a final transaction block, an un-finalized
transaction chain impedes consensus by preventing other miners from completing the chain. In this case,
miners will create a fork which follow the latest completed chain.

### Test-net update
These architectural changes will not incur any more delays on the alpha release since they require removing code rather than writing new code. Most of the current written modules can easily be used for this architecture. To give an updated estimate, I would say that very early next year the core will be up and running.

### Documentation changes
The state of the documentation about the core protocol is both incomplete and lagging behind at this point due to limited time available for research and development. However, as things get stabilized the documentation will become better and better with time.

### Summary
This development update is probably the most important one to date. It reflects a pivoting moment for the Purple Protocol with several architectural changes and simplifications. We have presented a new consensus model which has an exponentially smaller attack surface than the previous one. We are also nearing the anticipated public test which will show all remaining problems with the protocol.

### Thank you
I would like to personally thank the community for sticking around in times of uncertainty. You are the reason that keeps me going through both good and bad. I hope that this update sheds more light for everyone on what the current state of affairs are and where we are headed.

### Join Us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread The Word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!