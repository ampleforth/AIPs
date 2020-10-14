---
aip: 5
title: Sigmoid Supply Policy
author: Evan Kuo (@statuskuo), Ahmed Naguib Aly (@naguib)
discussions-to:
status: WIP
created: 2020-10-13
requires (*optional): N/A
---

## 1. Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Simply describe the outcome the proposed changes intends to achieve. This should be non-technical and accessible to a casual community member.-->
The current linear supply policy predisposes the Ampleforth network to short periods of rapid expansion and long periods of gradual contraction. This document proposes an update to the Ampleforth supply policy that would:

* Balance the time spent between expansion and contraction
* Converge on the price-target more quickly for minor deviations

## 2. Abstract
<!--A short (~200 word) description of the proposed change, the abstract should clearly describe the proposed change. This is what *will* be done if the AIP is implemented, not *why* it should be done or *how* it will be done. If the AIP proposes deploying a new contract, write, "we propose to deploy a new contract that will do x".-->
We propose to deploy a new contract that updates the current linear supply policy, with a modified sigmoid-shaped supply policy. 

## 3. Motivation
<!--This is the problem statement. This is the *why* of the AIP. It should clearly explain *why* the current state of the protocol is inadequate.  It is critical that you explain *why* the change is needed, if the AIP proposes changing how something is calculated, you must address *why* the current calculation is innaccurate or wrong. This is not the place to describe how the AIP will address the issue!-->

At present, the Ampleforth supply policy takes a `VWAP` as its input and offsets price differences of `X%` with supply changes of `(X%/rebase_reaction_lag)`. Two things to note about this. 

#### 3.1. Price ranges are asymmetric: 

- Expansion occurs in the price range of [1, ∞] 
- Contraction occurs in the range of [0, 1]

#### 3.2. Geometric expansion and contraction are “absolutely” different:

- When price is held constant above the target (expansion) the relative change in supply is constant, but the absolute change in supply grows geometrically. This means the change in absolute potential sell pressure (as measured in dollars) grows geometrically. 

- When price is held constant below the target (contraction) the relative change in supply is constant, but the absolute change in supply shrinks geometrically. This means the change in absolute potential sell pressure removed (as measured in dollars) shrinks geometrically. 

The key takeaway here is that expansion often rapidly outpaces contraction, resulting in prolonged corrective periods. 


## 4. Specification
<!--The specification should describe the syntax and semantics of any new feature, there are five sections
1. Overview
2. Rationale
3. Technical Specification
4. Test Cases
5. Configurable Values
-->

### 4.1. Overview
<!--This is a high level overview of *how* the AIP will solve the problem. The overview should clearly describe how the new feature will be implemented.-->
The smart contract upgrade replaces the current linear supply policy with a "mirrored" sigmoid-shaped curve that: 

* Asymptotes horizontally away from the origin. 
* Steepens rates of change near the origin. 

Horizontal asymptotes eliminate the [0, 1] vs [1, ∞] range problem of heavy-tailed markets. Moreover, when rates of change happen more aggressively near the origin, the network can converge on the price-target more quickly for minor deviations.

### 4.2. Rationale
<!--This is where you explain the reasoning behind how you propose to solve the problem. Why did you propose to implement the change in this way, what were the considerations and trade-offs. The rationale fleshes out what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->

Below we'll quickly review the basic sigmoid, and then explain the rationale for the "mirrored" sigmoid proposed.

#### 4.2.1. Basic Sigmoid
The basic sigmoid takes the following shape, note the presence of horizontal asymptotes and maximum slope near the origin:

// insert chart of basic sigmoid here (x-axis = price, y-axis = supply_change_factor)
  
**_Equation and Parameters_**

The basic sigmoid equation accepts as its input, `x`, the normalized difference between `VWAP` and the price target. It returns, `Y`, the corresponding supply change percentage.

```
Y = supply change %
x = normalized price deviation
```

<img src="https://assets.fragments.org/aip/sigmoid_basic.png" alt="drawing" width="380"/>

It also has shaping parameters that determine the location of lower / upper horizontal asymptotes, and the slope of the curve around its origin.

```
L = lower asymptote
U = upper asymptote
B = growth rate
```

#### 4.2.2. "Mirrored" Sigmoid

Although the basic Sigmoid is a good start, we can improve upon it by scaling supply changes such that they “mirror” one another. More specifically:

* If a demand-change-factor of `A` corresponds with supply-scale-factor `B`.
* We want to enforce that a demand-change-factor of `1/A` corresponds with a supply-scale-factor of `1/B`. 

This way, supply reactions to equal and opposite relative changes in demand, always execute in the same amount of time. To help explain, let’s walk through the simple example of an alternating series. 

**_Alternating Expansion and Contraction Example_**

Imagine Price alternates between $0.5 and $2, every 24hrs, infinitely:

<img src="https://assets.fragments.org/aip/series.png" alt="drawing" width="380"/>

In this case we want the magnitude of supply changes upon expansion and contraction to perfectly offset one another. Otherwise, if the magnitude of supply changes on expansion and contraction differ, there will be supply “drift” in one direction or another and the change in total supply will be unbounded over time.

**_"Mirroring" Solution_**

To accomplish this, let’s observe that:
* <code>For every scaling factor S, there exists an inverse scaling factor S<sup>-1</sup> such that S * S<sup>-1</sup> = 1</code>

And let’s also observe that:

* <code>For every price P there exists an inverse price P<sup>-1</sup> such that P * P<sup>-1</sup> = 1</code>

We can enforce the constraint of “mirrored” supply-change-factors by computing contraction supply-change-factors as the inverse of expansion supply-change-factors. In other words: 

<img src="https://assets.fragments.org/aip/mirroring.png" alt="drawing" width="380"/>

This way, for every price pair <code>{P, P<sup>-1</sup>}</code> the corresponding supply-change-factor pair <code>{S, S<sup>-1</sup>}</code> upholds the constraint that  <code>S * S<sup>-1</sup> = 1</code>.

// insert chart of mirrored sigmoid here (x-axis = price, y-axis = supply_change_factor)

**_Equation and Parameters_**

Recall that the basic sigmoid equation accepts a price deviation and returns a supply change percentage. Combining it with the mirroring equation gives the output: 

<img src="https://assets.fragments.org/aip/sigmoid_mirrored.png" alt="drawing" width="380"/>

#### 4.2.3. "Mirrored" Sigmoid vs Linear

We expect that the “mirrored” sigmoid supply curve will cause the Ampleforth network to  spend a more balanced amount of time between expansion and contraction, and avert prolonged contraction periods. 

// insert chart of basic sigmoid + linear here (x-axis = price, y-axis = supply_change_factor)


### 4.3. Technical Specification
<!--The technical specification should outline the public API of the changes proposed. That is, changes to any of the interfaces Ampleforth currently exposes or the creations of new ones.-->
TBD

### 4.4. Test Cases
<!--Test cases for an implementation are mandatory for AIPs but can be included with the implementation..-->
TBD

## 5. Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
