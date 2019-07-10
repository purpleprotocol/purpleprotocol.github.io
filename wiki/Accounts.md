---
layout: page
title: Accounts
wikiPageName: Accounts
menu: wiki
---

### Introduction
There are two main ways in which state can be stored on a decentralized ledger, the `UTXO Model` which is used by Bitcoin and the `Account Model`. In Purple, the `Account Model` is used. This is because Purple is first and foremost a decentralized applications platform, which is inherently stateful and because the `UTXO Model` is fundamentally stateless.

### Differences between the UTXO and Account models
In the `Account Model`, transactions that alter the ledger state are represented by operations, which incrementally compute an ephemeral state that is local to each participating node. This is in stark contrast towards the `UTXO model`, in which the state is encoded in the transactions themselves.

If in the `UTXO model`, the balance of an address is equal to the amounts of the unspent transaction outputs, in the `Account Model`, balances are simple numbers that are written to the local state. To illustrate both of them:

* `UTXO` - You have an unspent transaction output that is worth 10 coins. You want to transfer someone 5 coins. You create two outputs, both of which have 5 coins, one of which you give to the person you are transferring the coins to and one is for you. If the other person then transfers you back 3 coins, he will do the same and now you will both have two unspent outputs. The sum of the unspect outputs represent your respective balances.

* `Account` - This model is much less abstract than the `UTXO Model` and more easy to comprehend, mainly because it is very similar to how ledgers in banks work today. If I have 10 coins, my account balance says that I have 10 coins. If I transfer someone 5 coins, then both our balances will be 5. If he transfers 3 coins back, the balances will be updated accordingly. There is no concept of remaining "change" of a sent amount in the `Account Model`.

### Accounts in the Purple Protocol
There are two types of accounts and two corresponding types of addresses in the Purple Protocol:

* `Normal account` - This account type is usable by people and its representation in the ledger state is only a nonce, denoting the number of transactions sent by the account and the balances of each owned asset.

* `Contract account` - While a contract account has an owner, which is a normal account

#### Account addresses specification  
An account address is a [base58](https://en.wikipedia.org/wiki/Base58) encoded 33 bytes long string. The first always byte denotes the type of the account that the address represents.

#### Generating a normal address
In order to generate a normal address, we first have to generate `(PK, SK)` which is explained [here](https://pynacl.readthedocs.io/en/stable/signing/#algorithm). Then the address is simply the byte `1` which is the address type followed by the hash of the public key: `blake2b(PK)`.

#### Generating a contract address
A contract's address is generated in the following way:
1) Take the current nonce of the creator, encode it to 8bytes in BigEndian form.
2) Append to the encoded nonce the owner's address, followed by the code of the contract and then by the default state of the contract.
3) Compute `H = blake2b(M)` where `M` is the aforementioned byte string.
4) The contract's address is the byte `2` followed by `H`.
