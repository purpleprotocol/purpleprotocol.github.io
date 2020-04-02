---
layout: post
title:  "Purple Turns One Year Old Today"
date:   2019-03-01 13:24:41 +0300
author: Octavian Once
categories: purple update
description: One year ago today, I began dedicating my full available time on researching about the scalability issue...
---

One year ago today, I began dedicating my full available time on researching about the scalability issue in public blockchains and on building the Purple Protocol. What a challenging and incredible year this was!

###  Background
Back in early 2017, before I was even beginning to consider working on a blockchain project, I was a simple but curious hacker experimenting with distributed, concurrent systems and the functional paradigm. I was quite attracted to the idea that a well built distributed system could scale linearly and use all of the available computing resources in an efficient manner. 

One thing led to another other, and I found myself reading about subjects such as master/master database replication and how this brings an incredible improvement in performance with the caveat of having a much higher difficulty in architecting the mechanism in which the database nodes sync and keep track between themselves a.k.a. **how they achieve consensus**. 

#### The party

While I was lost in my efficiency haze, unaware of what was going on in the world, the ICO bubble was waiting to happen. The turning point was the party of my former employer, which was in the last week of August where I met quite a peculiar developer. Mind that this wasn't an ordinary party, I believe around 70% of the people there were developers or engineers while the others were either designers or had indirect ties with the technical field.

Event though everyone was talking about the latest APIs, tools, tech trends and gossip, this one guy stood out from the rest in what he was saying. I first began talking to him because I observed that he was seeming to push people away from him with what he was saying but he seemed to have a kind of glee in his eye while saying things so it stirred my curiosity.

He said his name was Lucian and he was talking about... well, you can guess it... Of course he was talking about blockchain! He began telling me about the lightning network and how it will speed up Bitcoin by a thousand fold and about how it is absolutely impossible to build a fast consensus mechanism and how off-chain solutions are ultimate, etc.

This seemed as utter nonsense to me at the time. So after I understood the mystery of the situation, and after a few rounds on myself of talking about the latest trends in functional programming which seemed to bore him a bit, we both went to talk to other people.

I didn't even realize it at the time, but a few questions began popping up in my head retrospectively. Things like "How can it be impossible to built a fast consensus algorithm for Bitcoin?" or "Didn't people say these things about airplanes or spacecraft?". 

At this point I still knew **virtually nothing** about Bitcoin or cryptocurrencies or Blockchain. I just categorized it in the mental box of "Really Complicated Tech Things That I Do Not Know". But the conversation I had at the party combined with the fact that at the time I was actively reading about consensus in distributed systems made me read more into the subject of blockchain. 

#### ICO Bubble
Fast forward to December 2017, when the ICO bubble began to erupt, my level of interest in the area was still at the level of "An Intriguing Curiosity", but my research on consensus in distributed systems was still going. I believe there was a day in the first week of December when most of the people I know absolutely went nuts and were saying these crazy things about how this thing called Blockchain was going to save the world and to buy this and that and the other.

This took me by surprise because I wasn't actively keeping with the news at the time, but it made me open the news websites. There was just one word written everywhere: **ICO**.

I heard about ICOs before but I couldn't believe that they could have gotten that big that fast. This made me actively think about this area I believe almost instantly. There was certainly something going on in this sphere.

#### Just a side project
In order to develop a better understanding, I started reading things such as the Bitcoin whitepaper, the Ethereum whitepaper and hacking a mini-blockchain together in javascript.

Then the concept of smart contracts clicked inside my mind. I realized this wonderful tool can be used to build a future without corruption and greed fuelled theft at grand scales. There were just a few problems:

* At that time the only platform which you could use to run smart contracts was Ethereum.
* Ethereum didn't allow users to use their favorite programming languages to write smart contracts. Instead they must learn Solidity, which is a worse version of javascript, agh.
* Because Solidity is a worse version of Javascript, the smart contracts that are written in it are **very** prone to faults, something which you really don't want in this kind of software.
* Even if this didn't matter,  the costs for running a simple piece of code were humongous. 
* Even if I had the money for it and I could navigate around Solidity's poor design, this still didn't matter since the platform itself was utterly slow and prone to faults.

So I started looking for another platform, but guess what, there wasn't any other out there. So I said to myself: "Surely something better than this can be done, why let this wonderful concept be locked away by poor design decisions?" and "This looks like a good challenge". After this I began researching and experimenting with various alternatives of achieving fast consensus in a public setting.

At some point I realized that if I didn't do this, someone might do it improperly and cause more and more damage to the reputation of these systems. On the 1st of March of 2018 I officially quit everything and went head on in tackling this massive challenge.

### Difficulties that  I had along the way
The first months were filled with self-doubt and weird reactions from my peers, family and friends. They all thought that I was going nuts. I thought that I was going nuts. And everything that was happening was nuts.

Here I was, a 20 year old software engineer with just a bit of experience from working at a firm that had startups as clients tackling this massive endeavour which makes even the most proficient of engineers or system architects to wet their undies. But I had 3 things on my side: the ability to learn very quickly, the ability to write software and nothing to lose.

I was also living with my parents so I could afford to just sit and think all day long about crazy technical things.

### This Year's Progress
That was the beginning of the iteration process which at this point has gone beyond any hope that I ever had when I first began this. A day before writing this, I finished and established the design of the memory layout of the virtual machine that will be responsible for executing smart contract code on the Purple Protocol. This will be released in the following weeks in the wiki of the protocol. All I can say is that I am very proud of the safety characteristics of the protocol's virtual machine.

In October 2018 the prototype code processed the first ever transaction on a private test-net with an average throughput of **5000 transactions per second**. This was the first moment since I started this that I stopped feeling self-doubt.

In November 2018 I started re-writing all of the code in the systems programming language Rust in order to squeeze every ounce of performance that can be spared. This is the point that we are in right now.

### The road ahead
We have come a long way and there is still a long way to go. Even at this point, there is no legal representation of the Purple Protocol, which has been made difficult by the crypto-winter. All that you read about in this blog post and more happened with **absolutely 0 funding**. The design of the protocol is more and more stable each day and I am writing code as much as I can, however there is so much one person without resources can do. 

In the previous days, we started a crowdfunding campaign with the hope that there are people in this world who have the common dream of ending financial tyranny and of building a decentralized asset transfer processor. If you can help, you would mean the world to us at this point. Unfortunately we cannot promise any rewards other than our maximum gratitude for helping us at this point because the legality of ICOs combined with the still somewhat experimental nature of the protocol makes us refrain from making promises that we cannot fulfil. With that being said, if at any point we introduce rewards for donors, this will be applied retroactively as well, meaning that donating at any time makes you eligible for future rewards.

Read more about this on our [crowdfunding campaign](/blog/participate-in-purple-crowdfunding-campaign). Thank you in advance if you choose to support us!

If everything goes according to plan, the launch should happen somewhere in late 2019. All I can promise is that I will keep building, day in and day out, same as before.

### Thank you
I would like to thank all of the people which have supported and helped me and the project along the way! Love you all!

If you find this interesting please share it further so other people can enjoy it as well!