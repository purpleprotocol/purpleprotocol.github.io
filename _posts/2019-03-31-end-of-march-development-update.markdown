---
layout: post
title:  "Purple Protocol End of March 2019 Development Update"
date:   2019-03-31 13:24:41 +0300
author: Octavian Oncescu
categories: purple development update
description: For the past two months I have been designing and working on the two of the most important modules in the Purple Protocol...
---

For the past two months I have been designing and working on the two of the most important modules in the Purple Protocol: The PurpleVM, which is responsible for executing smart contract code and the Rust implementation of SSPoW which is the core module of the protocol and is responsible for achieving consensus on the ledger state.

Today I have finished implementing the first part of the consensus algorithm which is responsible for assembling the graph that will be analysed in order to achieve **finality**.

### Background
For those of you who don't know, the current implementation of the Purple Protocol is a complete re-write from Elixir to Rust and this began in November 2018. 

The reason for the switch was the fact that I gotten to the point where I couldn't optimize the consensus algorithm any further because of the limitations of the language (no mutability, no pointers and garbage collection). At some point I began caching events and using high-level references in order to avoid copying and this seemed pointless. 

I always knew that the protocol was going to be written in Rust at some point but in the early stages where I needed to experiment a lot Rust would have proven to be cumbersome. My rationale was that the MVP was going to be written in Elixir and then we would copy the logic to Rust once it was established.

Another part of my rationale was that if the MVP which was written in a slow language and without any thought for optimizations actually ran and provided decent scalability properties then the version in Rust would prove to be exponentially better.

Now, I have gotten to the part of the code which will test those assumptions. It is still a work in progress so no benchmarks have been performed yet but I hope this will be over soon and a minimum testnet can be launched.

### Main Updates
Long story short, this is what has been brewing:

* The assembly of the Causal Graph responsible for holding the pending state of the ledger is now O(log n) instead of O(n^2), where n is the number of current pending events. This should bring quite a boost in transactions per second since the assembly algorithm is executed on each received event.

* The PurpleVM can at this point execute and validate basic bytecode which has basic control flow constructs such as `If/Else` and `Loop` constructs.

* The PurpleVM can execute and validate bytecode which uses code from functions that are declared in the same module or imported from other modules (other listed smart contracts).

* The heap of the PurpleVM will have a fixed size of 256^2 memory locations each with a maximum total of 8 bytes of possible storage. This makes the total heap size 524288 bytes in total (500kb). This is more than enough for smart contracts and the upper bound removes the need for charging gas on heap allocation, thus reducing fees. 

### What's next

I am thinking right now more and more about reducing smart contract fees as much as possible in order to foster usability and adoption and there will be several updates about this coming up. 

I also hope that after I finish the first iteration of the consensus module and after tying it to the network layer we can have a basic open testnet set up for everyone to check out.

Stay tuned! 