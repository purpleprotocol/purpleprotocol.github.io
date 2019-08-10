---
layout: post
title:  "Purple Protocol July 2019 Development Update"
date: 2019-08-06 13:24:41 +0300
author: Octavian Oncescu
categories: purple development update
description: In this month's update we are going to present a feature of the Purple Protocol that allows something that has never been done before on a decentralized ledger...
--- 

In this month's update we are going to present a feature of the Purple Protocol that allows something that has never been done before on a decentralized ledger: **the ability for regulations to be applied in a decentralized setting**.

### Rationale
One of the many problems and sources of confusion in the blockchain space today is the **inability of governments and regulators to migrate or create financial systems to a blockchain infrastructure without breaking the decentralized nature of the underlying blockchain**.

In simple words, today's blockchains are a good way to transfer assets in a decentralized way but are completely incompatible with existing financial systems.

### The Gap
The gap between regulators and early adopters is real and is widening each day. This is why the following things are currently happening:

* Governments banning decentralized cryptocurrencies.
* Existing players trying to get into the cryptocurrency space and being shut down by regulators as is the case of the Libra project proposed by facebook.
* Both early adopters and institutional investors are eager to get into the space but are having difficulties satisfying regulators.
* General distrust from regulators in regards to decentralized blockchain technology and its promises.

And the list continues... 

This is all a result of the incapability of current implementations to relate to our actual systems. This problem stems mostly from **the oracle problem**.

### The Oracle Problem
The oracle problem is found when one is attempting to write a smart contract that depends on data that is not found on the chain running the smart contract. Either someone or another program has to input the data into the chain in order for it to be accessible from a smart contract. 

This also means that the entity that inputs the data must be trusted with the validity of the data. This creates the paradox of relying on a third party in order to not rely on a third party.

However, in practice, this can be overcome by relying on an aggregate of multiple third parties, perhaps on a market model e.g. a smart contract that aggregates data and pays third parties according to the quality of the provided data.

So we can see that there are practical solutions to the oracle problem. Then why is it that the gap still exists? The answer is that even though there are conceptual ways to implement oracles, the chains of today are not built with this use-case in mind.

### Ethereum, Re-entrancy and Synchronization Primitives
This section is a deeply technical one and can be skipped by non-technical readers but is important in understanding why oracles don't work today and how we tackle this issues when building the Purple Protocol.

The problem underpinning the [famous DAO attack](https://www.coindesk.com/understanding-dao-hack-journalists) in Ethereum is also the main problem that underpins competent oracles, at the protocol layer.

The problem is actually two-fold:
* **Re-entrancy** can allow an attacker to potentially alter the internal state of a contract in a way that it was not designed to be changed. This happens when we have a circular dependency between several contracts i.e. contract `A` depends on contract `B` which depends on contract `C` which, in turn, depends on contract `A`.

*  When performing state changes on a contract that requires multiple, separate transactions altering the state, there is no built-in protocol for atomic synchronization in this use-case which is obviously happening asynchronously.

These two problems, when combined, result in an avalanche of unexpected bugs in a system that is supposed to be essentially bulletproof. Also, the lack of a way to perform synchronization essentially prevents competent oracles from being built.

The first problem is easier to visualize and solve, but there also exists a simple answer to the second problem as well. 

### How these problems are handled in the Purple Protocol
The answer to the first problem is simple: disallow circular dependencies between contracts at the contract validation layer. This is a safe default that prevents developers from shooting themselves (and others) in the foot.

The second problem instead, is solved using a pattern that has been used in low-level synchronization primitives for decades now: the `Read/Write Lock Pattern`.

The `Read/Write Lock` or `RwLock` synchronization pattern is used in low-level programming to allow either **many readers of a shared resource to immutably read the resource** or a **single writer that is allowed to modify the resource**.

This was the essence of the DAO bug: a multi-transaction write operation was performing a race condition with other read operations and was resulting in undefined behavior.

By using the `RwLock` pattern, we can effectively solve this issue by requiring contract methods that modify the state to hold a write lock on the state. **Read transactions will only be valid once the lock has been released, either by the writer in a another transaction or implicitly after a number of blocks have passed**.

### Asset Transfer Conditions
Now that we have presented the underlying primitives that allow oracles to exist in the Purple Protocol, we can present one of the main things that we can do with them: **allow or deny asset transfers at the protocol layer based on a predicate on an oracle result**. 

In other words, when we create and list a new asset on the Purple Ledger we can provide a programmatic transfer condition that will determine if any transfer using that asset can proceed.

For example, let us suppose that we want to move the US dollar on top of the Purple Protocol. We could **embed an asset transfer condition that only allows you to transfer or receive the asset if you have performed KYC using a third-party service**. 

**An oracle will verify the KYC status of your address and either allow or deny the transfer**. 

The beauty of this design is that centralized entities can leverage the benefits of blockchain without undermining its technical foundation and maintain compatibility with completely decentralized assets or contracts.

### Purple Core Development Status
The development of the core protocol is moving forward each day but at a justifiably slow pace. However, we have surely passed the insanely hard parts of the development process.

It is hard to exactly pinpoint the moment when the first full-fledged testnet will be ready. However, from my point of view I would say that we can begin testing on a network in the very near future.

### Summary
In this month's development update we have gone in some of the design decisions at the core protocol layer that allows us to interface smart contracts with the outside world via oracles. This feature allows regulators to effectively apply rules to a decentralized protocol without breaking its underlying characteristics.

### Join Us
If you believe that you can contribute to the development of the Purple Protocol, we encourage you to contact us! You can do so on our [discord](https://discord.gg/5ZVZnKd) or [telegram](https://t.me/purple_protocol) channels. 

### Spread The Word
Help us spread the word by sharing this article about the Purple Protocol!











