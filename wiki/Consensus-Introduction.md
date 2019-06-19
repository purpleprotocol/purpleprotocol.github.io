---
layout: page
title: Consensus Introduction
wikiPageName: Consensus-Introduction
menu: wiki
---

At the core of every blockchain protocol lies its consensus algorithm. The performance and safety properties of the underlying protocol are directly determined by the consensus algorithm and such it is the most vital subject that can be addressed.

This section's purpose is to explain the inner workings of the consensus algorithm that Purple Protocol uses.
 
### Introduction to Decentralized Systems
A decentralized system is first and foremost a [distributed system](https://en.wikipedia.org/wiki/Distributed_computing). A distributed system is a network of computers, called nodes, which are composite parts of a larger system. The difference between a decentralized system and a distributed one is that in a decentralized system, the composite nodes are also owned by different entities.

Every distributed or decentralized system must have a way to sync the nodes between themselves. A naive but effective way to solve this problem would be to require all participant nodes to sync with one central master node, which dictates the next state of the whole network. A good example of this is the [Master/Slave Architecture](https://en.wikipedia.org/wiki/Master/slave_(technology)) that is used in database replication. 

This works but has one big drawback: if the central master node fails, the whole network fails. This is not good in terms of security but can be managed in certain ways. Another drawback is that the master node effectively becomes a bottleneck for write operations throughput i.e. you can read stuff really fast from the database but it takes ages to write to it.

The solution to the write bottleneck is the [Master/Master Architecture](https://en.wikipedia.org/wiki/Multi-master_replication) in which you can write **and** read on any node in the network. While this makes the write throughput exponentially larger, it also increases the implementation difficulty exponentially. It is almost trivial to implement a master/slave architecture but you often require the smartest people on the planet to implement a master/master one.

Why is this the case? Well, imagine we want to implement a really fast database for facebook so we can retrieve **and** update our data really fast regardless of where we are on the globe. Only a master/master architecture can scale to this size but it introduces a big problem: if two people update the same thing from different parts of the globe at the same time, which one was first? The second update may be dependent on the first update and as such yield different results if the first update happened or not. In a master/slave scenario this is easy since the writes would happen simply in the order in which the master has received them and all the slaves will then sync their state to that of the master. 

Without a way to deterministically order updates between network participants, parts of the network would hold different records than the others, leading to potentially massive and disastrous results.

The algorithm which all of the participants execute in order to sync in distributed system is the **Consensus Algorithm** of that system. Its role is to determine the next state of the network in a scenario where there is no master node.

Now, we move to another level and add the caveat that this must happen also while part of the participants may be malicious and try to prevent the rest of the network from successfully syncing. This is the scenario which we care about since in a public and decentralized blockchain protocol, part of the nodes will **always** attempt to attack the network.

### Introduction to the Nakamoto Consensus Scheme
The first system to successfully achieve this feat i.e. staying alive and synced while people attack the network in a master/master scenario, is called **Bitcoin**. What is Bitcoin really but just a cleverly designed database? It simply holds records of addresses that own a certain amount of currency, which is also called bitcoin but with a lowercase `B`.

The [Nakamoto Consensus Scheme](https://blockonomi.com/nakamoto-consensus/) is the name of the consensus algorithm that is responsible for deciding the next state of the network. It is named after its mysterious creator, Satoshi Nakamoto. 

If we take the problem from the previous section, we can see how the Nakamoto Consensus Scheme solves this problem. Remember that we can either have a master/slave architecture or a master/master architecture. Bitcoin **uses a master/slave architecture** in order to sync the state of the nodes but it does so with another design decision which actually allows this to be effective: **the master node always changes**.

You only need a master node when the write happens and that is exactly what Bitcoin does. It chooses a new master node for **each write** in the database. This is done by requiring potential master nodes to participate in a lottery. These are the famous miners. They all compete for becoming master node by solving a very complicated mathematical puzzle of deterministic difficulty. This is called the **Proof of Work** which proves that a mathematical problem has been successfully solved.

A node with a valid Proof of Work becomes master node for a brief moment where he chooses the next transactions to approve and to write in the database. A batch of these transactions is called a **Block**.

This is pretty much how Bitcoin and all of the other cryptocurrencies work. While it is effective and safe, this consensus scheme suffers from one very big drawback: **it is slow, very slow**.

### Reasoning for developing another consensus scheme
While Bitcoin's Proof of Work provides ultimate security against attacks, it is also the main bottleneck of the network's transaction throughput. The most notable solution for this problem is called [Proof of Stake](https://en.wikipedia.org/wiki/Proof-of-stake) which works by handing over the responsibility for approving transactions to a handful of designated nodes. 

The way to become an approver in a Proof of Stake protocol is to deposit a large number of tokens to a chosen address. The reasoning is that if a designated node is malicious, it's deposit is taken away and such there is a contra-incentive for malicious behaviour. 

In theory, this sounds good. By moving the responsibility for validating transactions to only a handful of nodes, the transaction throughput of the network is greatly increased since consensus is faster among only a few nodes and they have more to lose for behaving maliciously than by being correct.

The reality is however, quite different. While it's reasoning is correct, the implementation of a Proof of Stake algorithm on a network has a big consequence: The inherent centralization of transaction approval which is a result of the simple fact that only a handful of nodes will ever have the required deposit to become a validator. 

A solution to this problem is the consensus algorithm of the Purple Protocol. It functions by using the same premise as Proof of Stake: **Consensus is faster among only a handful of designated nodes**. The only missing pieces then become the way in which those designated nodes are chosen and the way in which they are prevented from acting maliciously.

### Semi-Synchronous Proof of Work
The consensus algorithm of the Purple Protocol is an extension of the Nakamoto Consensus Scheme present in Bitcoin and all of the other iterations of it. The Semi-Synchronous Proof of Work Consensus Scheme (or SSPoW for short) is similar to the Nakamoto Consensus Scheme in the sense that they both choose a master node via the Proof of Work method. 

The difference lies in what happens after a master node is chosen. In Bitcoin, the master node chooses the next transactions
that are to be written in the ledger and then signs off, becoming again a slave. In Purple and SSPoW, when a node provides a valid proof, **it is then advanced to a pool of validators which together, act as the master entity in the system**. The validator selection is **synchronous** while the transaction validation is **asynchronous** thus the naming Semi-Synchronous Proof of Work.

There are several benefits of this:

* By decoupling transaction validation from validator selection we effectively remove the transaction bottleneck of Nakamoto Systems where transactions must wait for a valid proof.
* A master node can append **more than one block to the chain**.
* We can have more master nodes that concurrently validate parts of the pending transaction pool, increasing the volume of written transactions.
* We can finely adjust the pool size by increasing or lowering the difficulty of finding a valid Proof of Work thus we can have complete control on the safety aspect.
* We keep all of the benefits present in the Nakamoto Consensus Scheme such as the blockchain data structure and synchronisation via following the longest chain.

While not going into the deep details, this is the lifecycle of a validator in both Bitcoin and Purple/SSPoW:
#### Bitcoin
```
1. Find Proof.
2. Present Proof.
3. Choose the transactions in the next block.
4. Send the block to the network.
5. Go back to step 1.
```
#### Purple
```
1. Find Proof.
2. Present Proof.
3. Wait to be joined in the pool i.e. wait for the pool to finish approving designated amount of blocks.
4. Join the pool.
5. Wait until allowed to send a block of transactions (a simple explanation of this is that validators take turns, but it is more complex than that).
6. Choose the transactions in the next block.
7. Send the block the network.
8. If we still have an allocated share of blocks that we can approve, go back to step 5.
9. Issue a `Leave` event which states that we leave the pool and that we want our miner rewards to be written on a chosen address.
10. Go back to step 1.
```

As you can see the lifecycle in Purple is a bit more complex than that of Bitcoin and it introduces several new concepts such as the **allocated share of blocks** to both the pool and each of the individual validators. This concept is necessary in order to successfully secure the network. If we have too many validators, the network is safer but potentially requires more synchronization steps while if we have too little validators, the pool can potentially be attacked by malicious actors.

Each validator has allocated a share of blocks that it can approve on a single proof after which it must go back and find another proof. For example if node A has an allocated share of 10 blocks and it then sends a block it will have an allocated share of 9 events and so on, until it is consumed and it must leave. 

The validator pool itself also has an allocated share of blocks that it can approve before it must halt and join another node in the pool. In this way the state transitions are clear and we can finely tweak the safety and speed properties of the validator pool.

#### Ledger structure
With this method, the system needs to store **at least 2 blockchains** on disk: one for the blocks produced by new validators and one for blocks containing transactions. We can say then that the state of the transaction chain is directly dependent on the state of the validator chain. The interaction between these two chains can be thought of as a kind of **credit system** where the validator pool is credited with an allocated share of blocks for each valid block in the validator chain. For each block mined in the validator chain, the current pool is injected with both new validators and a new number of blocks that it can approve.

#### Validator Synchronization
Before blocks are distributed to the entirety of the network they must be passed through an additional ordering step in order to achieve finality and determine a strict order between them. Remember earlier when we explored master/slave and master/master systems and we found out that master/master systems are much faster but requires more exquisite synchronization? Well, if the entirety of the Purple system is a master/slave where the slaves are the listening nodes and the master is the validation pool. But if we separate the validator pool i.e. the master, as a sub-system then we can apply the same discrimination and we can say that **the validator pool is a master/master** system and as such it requires synchronization as well. After blocks pass this stage, they will very probably be written in the final state of the network.

The idea is that validators propose blocks for approval in an asynchronous fashion and their order is eventually determined. To do this we use a variant of [Logical Clocks](https://en.wikipedia.org/wiki/Logical_clock) that is suitable for dynamic scenarios and can be garbage collected. The only suitable candidate for this job is [Interval Tree Clocks](https://blog.separateconcerns.com/2017-05-07-itc.html). Their role serves that of a timestamp and is used to determine the final order of the blocks which are then sent to the rest of the network.

#### Interval Tree Clocks introduction
Because communication between validators is asynchronous and we need a way to determine the ordering of network events between validators that works well with an asynchronous medium. Using normal timestamps is brittle in this scenario since they can be easily manipulated and **they can drift between systems**. This is where [Interval Tree Clocks](http://gsd.di.uminho.pt/members/cbm/ps/itc2008.pdf) come in which enable us to establish a Partial Order on network events without requiring the use of timestamps.

An `Interval Tree` is a binary tree data-structure that **holds intervals**. `Interval Tree Clocks` uses this property to map the interval `[0, 1)` or any sub-interval of `[0, 1)` to a [logical clock](https://en.wikipedia.org/wiki/Logical_clock). Each sub-interval in an Interval Tree Clock is assigned to a participating node in a network, or in our case, to a validator from the pool. The logical clock associated with the respective share of the interval is the logical history of that node.

These are the operations that can be applied on an ITC stamp:
* `seed` - Initializes an empty itc stamp.
* `fork`- Splits the id component of a stamp into two stamps with identical clocks.
* `join` - Joins two stamps together, including their ids and clocks.
* `peek` - Retrieves the clock of a timestamp.

To illustrate how this works, let's suppose that we have 3 nodes A, B and C who want to participate in an asynchronous network. We must first have a setup phase where the interval [0, 1) is divided among the participants:
```
# We first create a seed stamp which has no 
# causal history and holds the whole [0, 1) 
# interval
seed_stamp = itc_seed()
 
# We then divide the seed between `A` and `B`
# which will be assigned the intervals [0, 0.5]
# and [0.5, 1) respectively
(A_stamp, B_stamp) = itc_fork(seed_stamp) 

# At this point we still need to join node `C`.
# We do this using a deterministic rule: fork
# the first node to the left with the largest
# interval share, which is `A`.
(A_stamp, C_stamp) = itc_fork(A_stamp)

# Now all of the nodes are mapped to an empty
# causal history. Their share intervals being
# the following:
A_interval = [0, 0.25]
C_interval = [0.25, 0.5]
B_interval = [0.5, 1)

# If another node joins, the same rule applies
A_interval = [0, 0.25]
C_interval = [0.25, 0.5]
B_interval = [0.5, 0.75]
D_interval = [0.75, 1)

# Now, let's suppose that node `A` broadcasts an event `E1`
A_stamp = itc_event(A_stamp)

# `A_stamp` now has the following property:
A_stamp > B_stamp and A_stamp > C_stamp

# When witnessing a broadcasted event, other nodes "merge" their 
# clocks with that of the broadcaster
B_stamp = itc_join(B_stamp, itc_peek(A_stamp))

# Now the stamps have the following property:
A_stamp == B_stamp and A_stamp > C_stamp and B 

# If node `C` however does not witness events from 
# other nodes and it decides to send an event, its
# causal history will make no sense when put against
# the other events.
C_stamp = itc_event(C_stamp)

# C's stamp is now un-related to all of the other stamps, and 
# we say that it is concurrent and no comparison can be made
A_stamp != C_stamp and C_stamp != B_stamp
```

#### Consensus Lifecycle - Joining Nodes
We have stated earlier that we must store at least 2 blockchains on disk at minimum: one for transactions and one for determining nodes which join the validator pool. In actuality, Purple will store 3 blockchains: one for buffering validators, called `EasyChain`, one for deciding the next epoch, called `HardChain` and one for storing the ledger state, called `StateChain`. 

This is because we want to join a larger set of validators in a single operation in order to refresh the validator pool and because it provides a way to deterministically pre-compute the assigned shares of the interval for each validator.

When miners successfully provide blocks for the `EasyChain` they are buffered until a block is mined on the `HardChain`. Then all of the miners are joined in one step. 

#### Epochs
What happens between the joins of validator sets is called an **epoch**. Each epoch has a designated validator set and an allocated amount of blocks that can be produced by the whole pool. Each validator also has an allocated number of events that it can produce, regardless of the current epoch.

Each epoch change represents a state change where new nodes are joined via the aforementioned method and old nodes (which have sent all of their allocated events) are retired and collect their rewards
