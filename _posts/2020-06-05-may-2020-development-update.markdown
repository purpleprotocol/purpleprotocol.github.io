---
layout: post
title:  "Purple Protocol May 2020 Development Update"
date: 2020-06-05 8:00:00 +0000
author: Octavian Once
categories: purple development update
description: This month has been a challenging month with many ups and downs...
--- 

This month has been a challenging month with many ups and downs for both the Purple Protocol and myself. I am writing this update a few days late as I am currently moving between apartments with very little time to spare. 

Last month has probably been the most challenging period of my life so far. For the first time since development began I was forced to stop writing code for the protocol as I had to work on other projects to sustain my living situation.

However, the development of the core protocol has not stopped. The other core developers are trying their best to make up for my current absence and I am only fulfilling an advisory role at this time. 

### Development status
A few days ago, we merged the last major pull-request required for the memory pool module to function. The virtual machine is also mostly operational. We have even **written simple assembly-level programs and ran them through the VM**. We did this in order to perform benchmarks. Here are some results:

![VM benchmarks](/assets/img/vm_bench.png)

We used an imperative version of the fibonacci sequence algorithm to benchmark the code. As you can see, the results are impressive: it took roughly **one millisecond** for the algorithm to calculate the fibonacci sequence to the 100th element. 

Here are some [benchmarks](https://github.com/drujensen/fib#optimized) in most programming languages for comparison. Those benchmarks do use the recursion variant of the algorithm which does make the comparison *a bit* unfair. However, the results are still impressive, the PurpleVM executes bytecode at native speed and outperforms all of the comparison benchmarks.

If you would like to check out the benchmark code yourself, visit the [official repository](https://github.com/purpleprotocol/purple/blob/master/src/purple_vm/bench/vm_bench.rs). If you would like to run the benchmarks yourself, simply clone the repository and run the following command `cargo bench -p purple_vm`.

### Next steps
All this means the major focus will now become the network layer which itself is partially complete. Once the network layer is operational, we can commence public network testing. 

Once we have a public network up and running, we will begin wallet development and start bridging compiler support.

This is what we will be focusing on the next few months.

### Summary
While I have been forced to take a more passive role in the development of the protocol, we are making steady progress towards the launch. Both the memory pool and the PurpleVM are more or less operational and only the network layer is still in the way.

I will focus more in this period on writing documentation and onboarding core developers. This is good as this part has been neglected in favor of developing the protocol itself. 

### Help spread the word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!