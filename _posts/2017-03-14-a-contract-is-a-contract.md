---
title: "A contrat IS a contract"
layout: post
date: 2017-03-14
categories: architecture programming c#
---

The concept of contract is not new, starting from abstract c++ class, going into interface in .NET, there is noting new under the sun.

We can discquisire about correctness, clarity and onesty of a contract, but a contract IS a contract.

The contract in software define how two systems interact, exchange information and/or provide service among them.

## Rule of thumb

When a class implements IDisposable is tell _"Hey... when you use me you have to call Dispose explicitily at the end!"_

