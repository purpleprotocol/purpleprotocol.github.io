---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
permalink: /
title: null
---

## What is Purple?
Purple is a scalable distributed ledger protocol designed for low-latency smart contracts and micro-transactions. It is especially suited for financial applications and the Internet of Things(IoT). Transactions are approved by connected devices which sign pending transactions and collect their fee as reward.<br><br> Purple's ledger structure and it's consensus algorithm enable transactions to be approved asynchronously, making the network's throughput increase with each device that is connected to it. 

## What consensus algorithm does Purple use?
Purple uses neither Proof of Work nor Proof of Stake in order to achieve consensus. It is instead achieved by using a novel algorithm which is best described as being a **decentralized clock**. The main feature of the decentralized clock is that it can generate proofs in the same time it takes to verify them and that it enables transactions to be approved individually and in parallel.

## How well does Purple scale when compared to Bitcoin or Ethereum?
Bitcoin can only process 3 to 4 transactions per second, Ethereum on the other hand can process around 15 to 20 transactions per second compared to the traditional payment processor Visa who can process around 1500-2000 transactions per second. This is because the network is bottlenecked by the proof of work algorithm. 

Purple on the other hand, because it is asynchronous, has a transaction processing rate which is in the order of the hundreds of thousands, maybe even more, making it suitable for the Internet of Things.