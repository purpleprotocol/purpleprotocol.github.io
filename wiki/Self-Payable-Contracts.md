---
layout: page
title: Self Payable Contracts
wikiPageName: Self-Payable-Contracts
menu: wiki
---

In order to try to alleviate the pressure on smart contract early adopters, Purple allows the payment of contract gas fees to be sustained by the contract owner. This is beneficial, for example, to applications which want to attract a user base or have a cost per execution that is already sustained by the user (such as buying an expensive item or service).

This kind of contract is called a **self payable contract**. The contract owner must choose at the moment of deployment if the contract will be a self payable one. In this case, the contract's gas limit and price must be adjusted programatically by the contract's code itself. 

### Potential problems
This kind of contracts are very similar to a mobile phone pre-paid card. If the contract runs out of money, it stops working. However, a problem becomes apparent: The contract owner can disable a smart contract if he extracts all of the funds from the contract's account.

For this reason, a self payable contract will become a user payable contract if it is unable pay the transaction fees. This ensures that the owner cannot disrupt the service of a contract at will. A contract will become again self payable once it is able to pay the execution fees.
