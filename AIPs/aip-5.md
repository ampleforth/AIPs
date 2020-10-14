---
aip: 5
title: Sigmoid Supply Policy
author: Evan Kuo (@statuskuo), Ahmed Naguib Aly (@naguib)
discussions-to:
status: WIP
created: 2020-10-13
requires (*optional): N/A
---

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Simply describe the outcome the proposed changes intends to achieve. This should be non-technical and accessible to a casual community member.-->
The current linear supply policy predisposes the Ampleforth network to short periods of rapid expansion and long periods of gradual contraction. This document proposes an update to the Ampleforth supply policy that would:

1. Balance out the time spent between expansion and contraction
2. Converge on the price-target more quickly for minor deviations

## Abstract
<!--A short (~200 word) description of the proposed change, the abstract should clearly describe the proposed change. This is what *will* be done if the AIP is implemented, not *why* it should be done or *how* it will be done. If the AIP proposes deploying a new contract, write, "we propose to deploy a new contract that will do x".-->
We propose to deploy a new contract that replaces the current linear supply policy, with a modified sigmoid-shaped supply policy. 

## Motivation
<!--This is the problem statement. This is the *why* of the AIP. It should clearly explain *why* the current state of the protocol is inadequate.  It is critical that you explain *why* the change is needed, if the AIP proposes changing how something is calculated, you must address *why* the current calculation is innaccurate or wrong. This is not the place to describe how the AIP will address the issue!-->

At present, the Ampleforth supply policy takes a **`24HR_VWAP`** as its input and offsets price differences of **`X%`** with supply changes of **`(X%/rebase_reaction_lag)`**. Two things to note about this. 

#### 1. Price ranges are asymmetric: 

- Expansion occurs in the price range of [1, ∞] 
- Contraction occurs in the range of [0, 1]

#### 2. Geometric expansion and contraction are “absolutely” different:

- When price is held constant above the target (expansion) the relative change in supply is constant, but the absolute change in supply grows geometrically. This means the change in absolute potential sell pressure (as measured in dollars) grows geometrically. 

- When price is held constant below the target (contraction) the relative change in supply is constant, but the absolute change in supply shrinks geometrically. This means the change in absolute potential sell pressure removed (as measured in dollars) shrinks geometrically. 

The key takeaway here is that expansion often rapidly outpaces contraction, resulting in prolonged corrective periods. 


## Specification
<!--The specification should describe the syntax and semantics of any new feature, there are five sections
1. Overview
2. Rationale
3. Technical Specification
4. Test Cases
5. Configurable Values
-->

### Overview
<!--This is a high level overview of *how* the AIP will solve the problem. The overview should clearly describe how the new feature will be implemented.-->
The smart contract upgrade replaces the current linear supply policy with "balanced" sigmoid-shaped curve that: 

#### 1. Limits rates of change away from the origin via horizontal asymptotes. 
This eliminates the [0, 1] vs [1, ∞] range problem of heavy-tailed markets. 

#### 2. Steepens rates of change near the origin. 
When rates of change happen more aggressively near the origin, the network can converge on the price-target more quickly for minor deviations.

### Rationale
<!--This is where you explain the reasoning behind how you propose to solve the problem. Why did you propose to implement the change in this way, what were the considerations and trade-offs. The rationale fleshes out what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->

<img src="https://assets.fragments.org/aip/sigmoid_basic.png" alt="drawing" width="70%"/>


### Technical Specification
<!--The technical specification should outline the public API of the changes proposed. That is, changes to any of the interfaces Ampleforth currently exposes or the creations of new ones.-->
TBD

### Test Cases
<!--Test cases for an implementation are mandatory for AIPs but can be included with the implementation..-->
TBD

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
