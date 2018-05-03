---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
permalink: /
title: null
---

## What is Purple?
Purple is a scalable distributed ledger protocol designed for running low-latency smart contracts. It is especially suited for financial applications and the Internet of Things. Transactions are approved by connected devices which sign pending transactions and collect their fee as reward.

## How well does Purple scale when compared to Bitcoin or Ethereum?
Bitcoin can only process 3 to 4 transactions per second, Ethereum on the other hand can process around 15 to 20. This is because the network is bottlenecked by the proof of work algorithm. 

Purple on the other hand, is asynchronous, meaning the network's throughput increases with each device that is connected to it. This makes it suitable for domains which require low response times such as the Internet of Things.

## What consensus algorithm does Purple use?
Purple uses neither Proof of Work nor Proof of Stake in order to achieve consensus. It is instead achieved by using a novel algorithm which is best described as being a **decentralized clock**. The main feature of the decentralized clock is that it can generate proofs in the same time it takes to verify them and that it enables transactions to be approved in parallel.

## Why not just use Proof of Stake?
Because Proof of Stake systems are centralized.

Only users of the network which own a substantial amount of tokens can approve transactions. If all of the approvers collude, they can approve transactions maliciously. 

This also means that when an approver's node is offline, it cannot approve transactions, making the network's throughput decrease.  