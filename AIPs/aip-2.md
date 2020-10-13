---
aip: 2
title: Continuous Vesting Token Distribution
author: Brandon Iles <brandon@ampleforth.org>, Nithin Ottilingam <nithin@ampleforth.org>
discussions-to: https://ampltalk.org/
status: Implemented
created: 2019-09-30
---

## Simple Summary

A smart-contract based mechanism to distribute tokens over time, inspired loosely by Compound and Uniswap.


## Abstract

The following is a smart-contract based mechanism for distributing tokens over time, inspired loosely by Compound's continuous interest accrual and Uniswap's liquidity pool tokens.

Distribution tokens (in our case AMPL) are added to a locked pool in the contract and become unlocked over time according to a once-configurable unlock schedule. Once unlocked, they are available to be claimed by users.

A user may deposit tokens (in our case AMPL-Uniswapv2 LP)to accrue ownership share over the unlocked pool. This owner share is a function of the number of tokens deposited as well as the length of time deposited. Specifically, a user's share of the currently-unlocked pool equals their "deposit-seconds" divided by the global "deposit-seconds". This aligns the new token distribution with long term supporters of the project, addressing one of the major drawbacks of simple airdrops.

When the user redeems, they receive their share of the current unlocked distribution pool along with their previously deposited tokens. Users may deposit or redeem at any time, so aren't required to "lock up" tokens for any fixed period. However, to disincentivize continuous deposit/withdraw behavior there is a gradual bonus that's applied that reaches max after a configurable time frame.

New distribution tokens may optionally be added to the locked pool by the owner along with an associated unlock time. However, the only way for distribution tokens to be removed from the system is by redeeming previously deposited tokens. User deposit tokens are only used to claim ownership and are never transferred to others.


## Motivation

As described in the [Token Transparency Report](https://medium.com/ampleforth/ampleforth-ieo-and-token-distribution-transparency-report-d7b632bbc838), roughly 20% and 23% of the total AMPL supply are currently allocated to the Treasury and Ecosystem Fund. We need to eventually find a way to distribute these tokens to users in “as sensible” way as possible. We’d also like to maintain our commitment to a rules-based policy in all aspects of the project. To that end, we can encode the distribution schedule and rules into a smart contract that operates autonomously over time.

Airdrops have shown limited success in aligning recipients and projects--many get lost, forgotten, or simply dumped on markets. This system shows proof of care through continued ownership through any market condition as well as rewards users who contribute positively to the ecosystem through providing liquidity.


## Specification

This application will implement the EIP-900 Simple Staking Interface and will be general enough for use with any ERC-20 tokens. The deposit aka "Staking" token and distribution token may be the same or be different and are specified during initialization.

```solidity
interface ContVestTokenDist is ERC20Detailed, IStaking {

    event TokensUnlocked(uint256 stage, uint256 numTokens);

    constructor(address stakingToken, address distributionToken);

    function getStakingToken() public view returns (address);
    function getDistributionToken() public view returns (address);

    // Unlock schedule & adding tokens
    function addUnlockStageTokens(uint256 numTokens, uint256 unlockTimestamp) public returns (bool);
    function numUnlockStages() public view returns (uint256);
    function unlockTimestampForStage(uint256 stage) public view returns (uint256);
    function unlockTokensForStage(uint256 stage) public view returns (uint256);

    // Pool info
    function getUnlockedPoolSize() public view returns (uint256);
    function getLockedPoolSize() public view returns (uint256);
    function totalDeposited() public view returns (uint256);
    function totalDepositedFor(address user) public view returns (uint256);

    function updateGlobal() public returns (bool);

    // EIP-900 Staking Interface for managing deposits and withdrawals
    event Staked(address indexed user, uint256 amount, uint256 total, bytes blockTimestamp);
    event Unstaked(address indexed user, uint256 amount, uint256 total, bytes blockTimestamp);

    function stake(uint256 amount, bytes _) public;
    function stakeFor(address user, uint256 amount, bytes _) public;
    function unstake(uint256 amount, bytes _) public;
    function totalStakedFor(address addr) public view returns (uint256);
    function totalStaked() public view returns (uint256);
    function token() public view returns (address);
    function supportsHistory() public pure returns (bool);

    function lastStakedFor(address addr) public view returns (uint256);
    function totalStakedForAt(address addr, uint256 blockNumber) public view returns (uint256);
    function totalStakedAt(uint256 blockNumber) public view returns (uint256);

    // ERC-20 Interface for internal CVTD tokens
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function name() public view returns (string memory);
    function symbol() public view returns (string memory);
    function decimals() public view returns (uint8);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function increaseAllowance(address spender, uint256 addedValue);
    function decreaseAllowance(address spender, uint256 subtractedValue);
}
```

## Rationale
Many different kinds of distribution have been attempted by projects. One thing that's clear is the limited effectiveness of airdrops. Historically, most airdrops have been either ignored by recipients or immediately sold.

Direct lockups would be the most straightforward, where a user locks up X tokens for Y time to receive X+Z tokens in return. This structure is easy to understand, but since we want to unlock in blocks there would be different tranches for tokens as they unlock through time. We'd end up having multiple concurrent airdrop contracts users interact with, which is confusing. Also, since there’s a limited number of total tokens to unlock, there’s a limited number of tokens that can be staked, so a single whale could conceivably take all the slots.

Projects are still free to place educational material on the frontend to teach users about the application before they interact with the underlying system. If done properly, this solves a second problem with airdrops related to user education.


## Implementation

The front-end is open to change during development, but the basic information to expose is:
- Number of AMPL staked by user
- Size of AMPL locked and unlocked pools
- Current fractional ownership of unlock pool
- "Stake" and "Unstake" functions
- Unlock schedule

Smart contracts will reside in https://github.com/ampleforth/token-geyser

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
