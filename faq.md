---
layout: home
title: FAQ
permalink: /faq/
---

## What is a smart contract?
A smart contract is a piece of code which has it’s own internal database and runs on every machine connected to the Purple Network. Once uploaded, it cannot ever be removed and it’s source code becomes public.

## What can I do with a smart contract?
Any algorithm that can be implemented on a normal computer can also be implemented in a smart contract. However, smart contracts are especially suited for tasks which require the following:
  * Public access
  * Fault-tolerance
  * Data permanence

Several areas which smart contracts are especially suited for include but are not limited to:
  * Healthcare
  * Public records
  * Machine learning
  * Decentralized applications

## What consensus algorithm does Purple use?
Purple uses neither Proof of Work nor Proof of Stake in order to achieve consensus. It is instead achieved by using a novel algorithm which is best described as being a **decentralized clock**. This enables the ordering of transactions in an asynchronous setting, enabling transactions to be approved in parallel.

## How well does Purple scale when compared to Bitcoin or Ethereum?
Bitcoin can only process 3 to 4 transactions per second, Ethereum on the other hand can process around 15 to 20 transactions per second which, compared to the traditional payment processor Visa who can process around 1500-2000 transactions per second is like comparing the speed of a remote controlled toy car to that of a bycicle. This is because the network can only be as fast as the slowest computer which is connected to it. <br><br> In Purple on the other hand, the transaction confirmation rate goes up with each computer connected to the network. This means that the transaction confirmation rate of Purple is in the order of the hundreds of thousands, maybe even more.

## How can I use Purple?
The main reason why smart contracts are not approachable today is the fact that they require a software developer to write and upload them to the network. This effectively prevents the vast majority of the population from harnessing the full potential of smart contracts.<br><br> We intend to launch, along with Purple, a service where software developers can upload contract templates which fulfill most of their common use cases. <br><br>For a non-technical person to use a contract template, he or she only needs to input the template's parameters and the code of the contract will be automatically generated. This is similar to how Wordpress has many website templates available written by different software developers and one can simply choose a template which fulfills his or her specific needs.<br><br> In case of custom or advanced smart contracts a software developer is still required to write the contract's source.

## How secure are Purple contracts?
Because the language used to develop smart contracts is typed and immutable, the contract’s code can be formally verified (it can be mathematically proved that it is correct).<br><br>   Immutability means that all of the bugs commonly found in mutable languages are ruled out from the start (mutable languages change data by changing variables, immutable languages don’t have variables, they use constants and change data by copying it to a new constant with the intended changes). While immutable languages are not popular in most of today’s software development circles, in the context of writing smart contracts, which cannot be altered after being uploaded to the network, immutable languages are the best fit since they provide many safety nets right from the start. Most bugs and security exploits come in today's smart contracts come from the fact that the programming languages used in their development are mutable.<br><br> Mutable languages are often preffered for two reasons: Performance and low developer cost of acquisition. This started in the 70’s when performance was paramount and software engineers were required to sacrifice security in order to achieve it. However, today’s world doesn’t need code that is optimized for performance since optimizations are done by compilers and today’s CPUs are extremely fast.<br><br> In today’s world where system breaches are almost on a day to day basis security is an absolute necessity in software and even more so in the context of smart contracts where a single bug could mean the loss of assets valued in the millions of dollars. Security is an area where immutable languages **shine**.  

## Will I be able to write contracts in Javascript or other widespread languages?
In the future, yes. The plan is to build a software development kit for most of the mainstream programming languages. 

## Can I mine on the Purple network?
Yes. Any computer connected to the network who verifies a certain transaction or smart contract execution receives it’s fee as reward. Since the number of transactions a node in the network can execute in parallel is only limited by it’s hardware and the amount of transactions flowing through the network at that given time, nodes with better hardware will receive more transaction fees.

## Is Purple open source?
Yes. You can follow the development of the Purple protocol on our github [ organization page](https://github.com/purpleprotocol).

## When is Purple going to be launched?
We intend to launch the Purple testnet in Q3 2018 and the mainnet in Q1 2019.

## Will there be an initial coin offering?
Yes. We will announce details about the ICO in the following weeks.