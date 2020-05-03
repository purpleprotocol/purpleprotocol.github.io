---
layout: post
title:  "Purple Protocol April 2020 Development Update"
date: 2020-05-03 8:00:00 +0000
author: Octavian Once
categories: purple development update
description: This month we have been continuing on focusing with the development of the network layer...
--- 

This month we have been continuing on focusing with the development of the network layer of the core protocol. This is one of the most complex parts of the protocol and therefore requires the most effort and time. The good part is that after this is done, there's not much else before we can start the phase 1 test-net.

By focusing on the network layer, we are now preparing to transition the project to the phase 1 test-net. This month's development update is about the latest news in the development of the protocol and what we plan to do in the next few months.

### The core development team has expanded
There was a huge stretch of time where I was the single active core developer, but this has now changed. In the past months, the core development team has expanded. We are now 3 active core developers and are onboarding the 4th.

While the progress we have made so far with so few people is [significant](https://github.com/purpleprotocol/purple/graphs/contributors), there is still much work to be done. 

We are still looking for contributors, if you would like to help, give us a [shout on discord](https://discord.gg/5ZVZnKd).

### The wallet development will start soon
Until now the protocol sustained lots of changes which made the development of a stable wallet close to impossible. Now, with more core developers joining the team and the protocol starting to become stable means that we will soon begin the wallet's development. Hopefully, we will have a working wallet when the phase 1 test-net begins. 

### PurpleVM compiler support through WASM
While the PurpleVM is becoming stable with most of the instruction set being implemented we have to start looking towards bridging compiler support for programming languages soon. 

The way we are planning to do this without writing a compiler for each target language is to leverage WASM. Since PASM is very similar to WASM but has a lighter encoding, we can simply compile WASM binaries to PASM. 

This would offer the benefit of **supporting smart contracts in any language that targets WASM while using the lighter encoding of PASM**.

This all means that you will be able to write smart contracts in languages such as Rust, Java or C# much sooner than expected. While we are not promising full-support for all languages from the start, this is something that can be achieved if everything goes smoothly.

### Summary
In this month's development update we have been talking about the latest developments in the Purple Protocol such as how our team has begun to expand and what we plan to do before the release of the phase 1 test-net.

It has been a long road that now feels like it is drawing to a close. The core protocol certainly feels more stable than a few months ago and while our goals of launching before 2020 were delayed, the amount of progress we have made so far is certainly incredible.

### Help spread the word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!