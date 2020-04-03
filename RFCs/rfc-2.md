---
rfc: 2
title: Disallow Contracts from calling Rebase
author: Ampleforth Dev Council - Brandon Iles <brandon@ampleforth.org>, Nithin Ottilingam <nithin@ampleforth.org>, Ahmed Naguib Aly <naguib@ampleforth.org>
type: Core
status: Final
created: 2020-03-26
deployed: 2020-04-02
---

## Simple Summary

Proposal to disallow rebase from being called by contracts, to guard against flash loan attacks on liquidity providers.

## Abstract

Flash loans can be leveraged in combination with a rebase call to effectively "steal" rebase awards from liquidity providers on trading and lending platforms. Liquidity providers can be negatively impacted on both expansionary and contractionary cycles. Without any change to address this, AMPL holders may be disincentivized from providing liquidity. This has a negative effect on the health of the AMPL ecosystem, and also counteracts the liquidity incentive program described in [RFC-1](RFCs/rfc-1.md)

A solution of preventing non-EOA addresses from calling rebase is proposed.

## Motivation

Consider the following scenario. Alice deploys a contract onchain. It has one method that performs the following:
1) Takes out all AMPL from uniswap v2 via a flash swap.
2) Call rebase when in an expansionary period.
3) Return AMPL back to Uniswap, keeping rebase awards.

While this does provide an incentive for callers to execute rebase, the net result is that liquidity providers lose out on all the AMPL tokens they would have received from an expansionary cycle.

Similar attacks can be performed with non-AMPL flash loans and don't require AMPL to be available on lending platforms.

1) Take out an ETH flash loan from Dydx, Aave, and/or other platforms.
2) Buy as much AMPL as possible from Uniswap, Kyber, Bancor, ...
3) Calls rebase when in expansionary period
4) Sells AMPL back
5) pays off ETH loan

Alice only has to overcome the following fees:
- 2*0.3% uniswap fee
- Dydx charges 0% on flash loans, Aave charges 0.09%

Alice is profitable as long as the oracle rate is roughly > $1.06. With the existing deviation threshold of 5%, positive rebases start at $1.06155

Shorts spanning contractionary rebases similarly impact liquidity providers by offloading more of the contraction adjustment to them.

The most damaging effect of allowing flash loan users to call rebase is that it will discourage people from supplying liquidity on DEXs and lending platforms. This would lead to unnecessarily high volatility and unpredictability around supply adjustments.

## Specification

To address this, the dev council proposes adding a single check at the top of our Supply Policy rebase function:

`require(msg.sender == tx.origin);`

This enforces that rebase is only called from EOA wallets, which removes the possibility of flash loans being used simultaneously with rebase calls.

Note that `tx.origin` is NOT used for authentication.

## Rationale
EOAs (Externally Owned Account) are unable to leverage flash loans, because flash loans require multiple steps within the same transaction: loan, do something, repay. Requiring rebases be called from EOA is the simplest way to bar flash loan attacks on rebases.

While it's still possible to perform the same actions without flash loans, the user would be subjecting themselves to considerable market risk and also have to put a lot of their own capital at stake to do so.

An alternative implementation would be to create a whitelist of ethereum addresses (EOA or contracts) allowed to execute rebases. There are two benefits: approved contracts (including multisigs) would be able to call rebase, and we would avoid using the relatively controversial `tx.origin`.

The downsides are that it would expand the surface area of governance, add more complexity to the system, and remove the ability for "anyone" to call rebase. So far, the Ampleforth system only uses whitelists in the oracle where it's inherently part of the architecture. We'd prefer to not expand the use of whitelists if not absolutely necessary, especially in the token and policy.

While enforcing EOA status breaks the general composability of contracts, we believe the proposed solution is the best tradeoff. Since rebase is meant to be "public", the use case for multisigs is basically non-existent, allowing all contracts is too dangerous, and whitelisting contracts brings too much governance requirements.

One common way of disallowing flash loans around a critical operation is to make that operation span multiple transactions. We could, in theory, require rebase to occur in two phases, each in different blocks. However, this is not viable in our case since one step must affect the supply adjustment and this adjustment could still be wrapped with flash loans.

## Implementation
The dev council will author the code change to the uFragments repo. The governance multisig will then be used to upgrade the policy's implementation code via the OpenZeppelin AdminUpgradeabilityProxy.

[Github Release](https://github.com/ampleforth/uFragments/releases/tag/v1.0.1), deployed 2020-04-02.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
