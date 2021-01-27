---
aip: 6
title: Additional interface methods to the token and policy
author: Nithin Krishna (@nithinkrishna)
discussions-to: https://github.com/ampleforth/AIPs/issues/31
status: Proposed
created: 2021-1-26
---

## Summary

The following additions to Ampleforth ERC20 and policy smart contracts are proposed.

* Elastic token interface methods `scaledBalanceOf`, `scaledTotalSupply`, `transferAll` and `transferAllFrom`. [#180](https://github.com/ampleforth/uFragments/pull/180), [#187](https://github.com/ampleforth/uFragments/pull/187)
* Support for `permit` EIP-2612 (gasless approvals) [#181](https://github.com/ampleforth/uFragments/pull/181)
* A convenience function (`globalAmpleforthEpochAndAMPLSupply`) on the policy contract for multi-chain integration [#190](https://github.com/ampleforth/uFragments/pull/190) [#189](https://github.com/ampleforth/uFragments/pull/189)
* Minor gas optimizations [#186](https://github.com/ampleforth/uFragments/pull/186), [#182](https://github.com/ampleforth/uFragments/pull/182)

<br/>

## Test Cases

Unit tests have been added to test the additions.

`scaledBalanceOf`, `scaledTotalSupply` and `globalAmpleforthEpochAndAMPLSupply` do not modify any smart contract state.

The `permit` implementation is identical to existing audited implementations on the Aave's [AToken](https://github.com/ampleforth/protocol-v2/blob/master/contracts/protocol/tokenization/AToken.sol#L268) and [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/drafts/ERC20PermitUpgradeable.sol).

`transferAll` and `transferAllFrom` are identical to the existing implementations of `transfer` and `transferFrom` on the AMPL ERC-20 token.

<br/>
