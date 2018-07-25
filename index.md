---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
permalink: /
title: null
---

## What is Purple?
Purple is a scalable distributed ledger protocol designed with support for smart contracts. It is especially suited for financial applications and the Internet of Things. 

## How well does Purple scale when compared to Bitcoin or Ethereum?
Bitcoin can only process 3 to 4 transactions per second, Ethereum on the other hand can process around 15 to 20. This is because the network is bottlenecked by the Proof of Work algorithm. 

## What consensus algorithm does Purple use?
Purple uses neither Proof of Work nor Proof of Stake in order to achieve consensus. A number of nodes called sequencers are responsible for processing pending transactions. A node becomes a sequencer by providing a valid Proof of Work on the last state of active sequencer nodes. In this way, sequencer nodes are selected in an unbiased and decentralized manner.

Sequencer nodes then achieve consensus among themselves by establishing a partial causal order between the transaction events that they send between themselves and then vote on which of the events are to be included in the ledger.

Pending transactions are processed by a faster consensus algorithm and the current sequencers are established using the Proof of Work algorithm which makes the network secure against attackers.

In this way, Purple offers the same security properties as Bitcoin while enabling a much higher transaction throughput.

## Why not just use Proof of Stake?
Because Proof of Stake systems are centralized.

Only users of the network which own a substantial amount of tokens can approve transactions. If all of the approvers collude, they can approve transactions maliciously. 

This also means that when an approver's node is offline, it cannot approve transactions, making the network's throughput decrease.  