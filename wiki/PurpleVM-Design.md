---
layout: page
title: PurpleVM Design
wikiPageName: PurpleVM-Design
menu: wiki
---

The Purple Virtual Machine or PurpleVM, is responsible for executing smart contract code in the context of the Purple Protocol. This page provides an introduction to the design of the PurpleVM and the reasonings behind it. This is a bit more technical and low-level so if you want a high level description of smart contracts, take a look at the [smart contracts introduction](https://github.com/purpleprotocol/wiki/wiki/Introduction) page.

## Introduction 
The role of the PurpleVM is to **sandbox software** that will run on the entire network. A naive way of implementing smart contract functionality would be to simply allow users to upload native code and for the network participants to run it locally on their computers, however this naive solution has the following **big** problems:
* The native code will be different on each target and processor, making it incompatible with a sub-set of participants.
* Native code can access the file system, so an attacked would be able to do harm to not just one computer in a network, but **to all computers** in the network.

This is why we **absolutely must** sandbox the uploaded software. Then a competent blockchain virtual machine is designed in such a way that it first and foremost cannot ever access any kernel functions of the underlying operating system and can also allow users to write competent programs in it by being a compiler target for one or more programming languages.

## Turing Completeness and the Halting Problem
Now, let us suppose that we build a competent virtual machine which cannot access anything at the kernel layer and it runs anything you can think of. There is only one thing that can crash the whole network at this point, and that is [the halting problem](https://en.wikipedia.org/wiki/Halting_problem). In simple words, the halting problem states that it **absolutely impossible** to determine if an arbitrary piece of software will run forever or not on any input that it receives.

Why is this a problem for smart contracts? Well, remember that smart contracts will run on **every computer** that is connected to the network. Now imagine that someone uploads a smart contract that contains an infinite loop. That means the loop will run **forever** on every computer that is connected aka the network has just been crashed. 

If the halting problem didn't exist, we could simply write an analyser which beforehand determines if that piece of code will halt or not. But **this is impossible** and has been proven to be so even before we even had physical computers.

We only have two other alternatives:
* **Remove Turing Completeness altogether**, only allowing Turing Incomplete smart contracts i.e. do not allow loops, only if/else statements and variable allocation. This makes the program to **always** terminate and have an upper bound in execution.
* **Simulate Turing Incompleteness in a Turing Complete context** i.e. placing an artificial upper bound on the code execution while also allowing loops. This is also called the Gas Model. It means that when a contract begins execution it has a limited amount of "Gas" which it can consume before it **must** halt, similarly to how a car works. The idea is to make the user pay for keeping the network working so that eventually, in the worst case scenario, the user will run out of money and the contract will definitely halt.

## Other implementations
Purple is not the first blockchain to implement smart contract functionality. In fact, you could even argue that Bitcoin has smart contract functionality and that it was the first one to do so. At the same time, the concept of smart contracts exists [since the 90's](http://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/smart_contracts_2.html). 

#### Bitcoin
Bitcoin defines a simple instruction set which users can leverage to construct scripts that execute on the Bitcoin blockchain. The caveat is that Bitcoin smart contracts are Turing Incomplete. As you can see, the creator/s of Bitcoin decided that Turing Completeness is rubbish in this context and decided to follow the first solution to the halting problem i.e. not allowing any loops.

#### Ethereum
The Ethereum project is the most popular blockchain system which implements smart contracts. It is also the first to follow the second solution to the halting problem, which is having a Gas Model. In Ethereum, you can more or less write **any kind of software** and it will run "fine". The caveat is that for each executed instruction, you have to pay a few sub-units of ethereum which are measured in gas units. We won't go into detail about this but this is pretty much how it works: when your coins/gas runs out, the contract execution stops. This solves the problem but it introduces another one: the expensiveness of actually running smart contracts, since **every instruction** costs a **fixed** number of gas which when put against the market might either mean that an instruction costs $0.000001 Dollars or $10 dollars. This is quite bad for usability. 

Imagine having to pay $1000 dollars (extreme case that will never happen I hope) just for running 50 lines of code on the Ethereum blockchain.

## The Purple Virtual Machine
We now present the PurpleVM and how it has been designed from the ground up in order to provide the best solution for both the halting problem **and** in order to at the same time provide a much better developer experience than any of the currently existing solutions. 

#### WebAssembly
In order to tell this tale, we must first speak about [WebAssembly](https://webassembly.org/) and the magnitude of its importance in our modern world. The WebAssembly format has been designed with only one purpose in mind: **run native code on browsers** aka its mission is to **replace javascript** as the de facto language for writing browser client applications.

Right now, browsers can only run Javascript, but with WebAssembly, we can run code written in most programming languages in browsers. This means that for example games, which are mostly written in C++ will be able to run in your browser, something which was not even remotely possible until now.

Now, why is this important in the context of smart contracts? Well, if we look at the webpage of WebAssembly, we see the following descriptions of WebAssembly: 
> WebAssembly (abbreviated Wasm) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable target for compilation of high-level languages like C/C++/Rust, enabling deployment on the web for client and server applications.

> WebAssembly describes **a memory-safe, sandboxed execution environment** that may even be implemented inside existing JavaScript virtual machines.

If you remember, previously we stated that we need a sandboxed execution environment that is as safe as possible while it would also be extremely beneficial to allow users to write smart contracts in programming languages that they **already know**.

WebAssembly provides **both** of these things while also supposedly providing memory-safety, something which is **highly** desirable in software like smart contracts.

#### Problems with WebAssembly
While WebAssembly does have things that are highly desirable for the context of Purple and smart contracts, it has several drawbacks:
* WebAssembly was designed for browsers, not for blockchains.
* The WebAssembly binary encoding is quite large and has many unnecessary things for blockchains. We need to cram in as much data as possible in a tight space and the binary format of WebAssembly does not help us here.
* The WebAssembly virtual machine has absolutely no notion of gas and this would require heavily modifying it for our purposes.
* Even though WebAssembly is described as being a memory-safe stack machine, **it is in fact not**. The WebAssembly memory model describes a set of [variables aka registers](https://github.com/WebAssembly/design/blob/master/Semantics.md#local-variables) which can hold memory instead of a stack. This is not exactly memory-safe and it certainly is not the attribute of a stack machine.

#### The best of both worlds 
What we want are the delivered or purposed benefits of WebAssembly which are a cross-platform sandbox environment which is memory-safe and that provides a compilation target for commonly known programming languages. However we also want a lighter encoding format and blockchain specific intrinsics such as opcodes for manipulating the ledger and the implementation of the Gas Model under the hood.

### PurpleVM Design
The PurpleVM instruction set or PASM, **heavily** borrows from the WebAssembly instruction set and has similar characteristics to it which are:
* A structured binary format that can be **statically validated prior to execution**.
* Four primitive types: `i32`, `i64`, `f32` and `f64`.
* High level control flow instructions such as the `Loop` and `Break` or `BreakIf` instructions.
* High level comparison instructions.
* High level math instructions.
* Bit manipulation instructions.

#### Differences between WASM and PASM
However, there are also several striking differences which are mostly intended to increase the safety and the validation capabilities of deployed code. These differences include:
* The memory model of the PurpleVM uses an actual stack instead of registers for storing local variables and has only 3 instructions for manipulating it which are `Push`, `Pop` and `Pick`. This means that we can determine **before we run the code** if the stack operations are memory-safe or not.
* The PASM binary format is an order of magnitude smaller than WASM since we do not need to inter-operate with javascript or to have a growable memory.
* The heap of the virtual machine is described as a two dimensional array of 256 elements each of which being able to store a maximum of 8 bytes. This provides an upper bound on heap allocation which removes the need to charge gas on heap allocations as it is in Ethereum. This means that the maximum number of data that you can store on the heap in Purple is roughly 524kb. More than enough for smart contracts. Using more than this would mean using 16bit indexes which would increase the size of the binary format.

#### Differences between the PurpleVM and the EVM.
While both the virtual machine of Purple and Ethereum are stack machines and both have a Gas Model and allow the execution of "Turing Complete" smart contracts, their similarities end here. 

The Ethereum Virtual Machine has the following characteristics:
* It has a very limited in size stack which can hold a bit more than a dozen local variables, you need to allocate to the heap beyond this.
* Each instruction, including heap allocation, costs a **fixed** amound of gas.
* You can make jumps to arbitrary positions in the code (hint: this is very unsafe).
* Integers are **only** 256bits in size, this means no compatible integer types with most programming languages such as i32, i64 etc. This means incompatibility with the rest of the world.
* Expanding on the previous point, since the assembly of the EVM has incompatible semantics with almost everything else, you must design **dedicated programming languages** for using it. The most commonly known of these is called Solidity, which is known to be a worse and less safe version of javascript.
* There is no code validation and the assembly format is unstructured.

In contrast, the Purple Virtual Machine has the following characteristics:
* While not being as large as, for example, the stack size of the JVM, the stack size of the Purple VM can hold at least a few hundred items.
* Stack allocation is free and memory-safe. 
* Heap allocation is free but has an upper bound.
* Integers that have the same size as common architectures, making it a valid compiler target for things such as [LLVM](https://en.wikipedia.org/wiki/LLVM). This means that you can potentially develop smart contracts in any of the LLVM compatible front-ends which include: C, C++, Java, C#, TypeScript, Rust, Haskell, Ocaml, and many more (compiled) programming languages. Unfortunately, execution of dynamically typed programming languages is not possible in this model or severely limited.
* The assembly is structured and can thus be pre-validated i.e. you must explicitly declare functions, loops, if/else statements etc.
* Function definitions **must** explicitly state the types of the parameters and return values, so that pre-validation is possible.

