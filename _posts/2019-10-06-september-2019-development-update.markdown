---
layout: post
title:  "Purple Protocol September 2019 Development Update"
date: 2019-10-06 13:24:41 +0300
author: Octavian Once
categories: purple development update
description: We have been focusing intensely in the past month on writing the network part of the core protocol...
--- 

We have been focusing intensely in the past month on writing the network part of the core protocol. At this point, I would say that it is around half-way done, after which we can release the first iteration of the Purple test-net. This is an incredibly exciting moment. 

### Ramping up the test-net
With most of the core modules now being more or less complete, we can begin thinking about medium to large scale network tests. This means that the whole community can now get involved and begin testing the core code.

Initially, some functionality will either be limited or not working and we do not expect it at this point to be otherwise. The goal of the test-net is to polish the existing implementation to the point where it is ready to be safely used by everyone.

### What is the exact timeline on this?
This can happen as soon as the end of October but as always, roadblocks can still appear along the way. One thing is certain and that is: We will have a test-net up and running before the end of the year.

### Will I be able to mine purple coins on the test-net?
Initially, no. The first phase of the test-net will be a preliminary phase where nothing is expected to work

##### Test-net phases overview
* **Phase 1** - No-strings attached test-net where nothing is expected to work. Mining purple will be possible but **no miner rewards will be retained in the main-net ledger**.
* **Phase 2** - Pre main-net where most of the things are expected to work but there are still issues to be discovered and solved. In this phase miner's rewards will be transferred into the main-net. The only caveat is that **roll-backs are possible to happen in this phase which might result in some miners losing their rewards**.
* **Phase 3** - Main-net launch, after which we will **no longer perform any state rollbacks**.

### Will I be able to develop and test Purple Dapps on the test-net?
While compiler support will be severely limited in the first and probably the second phase, it will still be possible to deploy dapps to the test-net. This means you can start building and testing your dapps before the main-net launch.

Once we enter **Phase 2** the focus of the development effort will turn towards bridging compiler support, developing the desktop and mobile wallets and polishing the existing architecture as much as possible.

### Summary
This month's development update is probably the last development update before we have a test-net up and running. We have presented the phases of the launch of the main-net in this update gave valuable information to individuals interested in mining on the Purple Protocol.

### Join us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread the word
Help us spread the word by sharing this article about the Purple Protocol! Stay tuned for more!

