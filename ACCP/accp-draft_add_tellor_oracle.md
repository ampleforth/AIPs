---
accp:
title: "Add Tellor oracle to provide AMPL/USD and US Personal Consumption Expenditures values"
author: Tellor Team (info@tellor.io)
discussions-to:
status: Proposed
created: 2021-05-21
requires (*optional): N/A
---

<!--You can leave these HTML comments in your merged ACCP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new ACCPs. Note that an ACCP number will be assigned by an editor. When opening a pull request to submit your ACCP, please use an abbreviated title in the filename, `accp-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the ACCP.-->
This is a proposal to add Tellor as an additional oracle to the Ampleforth system. The goal of this addition is to further decentralize the protocol and create a more robust, censorship-resistant price feed.  Tellor is a decentralized oracle that uses a network of staked miners who compete to fetch off-chain data and place it on-chain.

## Abstract
<!--A short (~200 word) description of the variable change proposed.-->
We propose that Ampleforth will use Tellor to bring on two values:  the AMPL/USD value and the US Personal Consumption Expenditures (a key measure of inflation). The values will be medianized with the current price feeds provided by Chainlink and the Ampleforth team.

## Motivation
<!--The motivation is critical for ACCPs that want to update variables within Ampleforth. It should clearly explain why the existing variable is not incentive aligned. ACCP submissions without sufficient motivation may be rejected outright.-->
In order for AMPL to do its daily “rebase,” it requires a price of AMPL/USD on-chain so that the protocol knows how to adjust the supply.  It doesn’t peg directly to the dollar, but rather an inflation-adjusted dollar.  Using the US Personal Consumption Expenditures Price Index, it can track the value of the US dollar versus costs.  This means that AMPL will still hold a stable value versus goods and services, even if the US dollar collapses in value. There is no reliance on traditional banks or lenders of last resort to guarantee liquidity.

As an oracle that specializes in decentralization and censorship-resistance, we believe adding Tellor into the system greatly helps Ampleforth’s own efforts of decentralization and adds further security and  robustness to the price feed.

## Specification
The supply change is referred to as supply rebase and is applied daily at 2 AM UTC. The price used to compute the deviation from the market price is the volume weighted average of AMPL trades over the 24 hours of the previous day from midnight to midnight UTC across all markets/exchanges and pairs.  

Tellor becomes the third oracle to power AMPL price feeds, joining Chainlink and Ampleforth itself. The median (or average, if there are only two) will determine the daily rebase to ensure the most accurate outcome each day.

## Rationale
In order for the Ampleforth community to fulfill their vision as a decentralized financial primitive, the censorship resistance of the protocol must be the first and foremost priority.  The communities of Tellor and Ampleforth have similar drives towards and beliefs as to what constitutes a decentralized protocol and the ability of the two projects to work together will help further the entire space.

Tellor’s system is a completely on-chain data feed that can be accessed by Ampleforth smart contracts.  The design for integrating Tellor into Ampleforth includes creating an adaptor that pulls values from Tellor and pushes them to Ampleforth's oracle medianizer, and configuring this adaptor as a new provider in the medianizer. Additionally, changes to Tellor are needed, which include an off-chain mechanism for both calculating the Ampleforth price and the CPI update as well as automated software for maintaining the liveliness of the oracle in a decentralized manner.

Tellor added the AMPL/USD price calculation in mid 2020 and has been updating it since and they have also created scripts for automating the price updates before they are needed by the AMPL system.

Tellor mining software (data providers) - https://github.com/tellor-io/telliot

Tellor tipping scripts - https://github.com/tellor-io/tipperTemplate

## Technical Specification
Tellor is a fully on-chain oracle, so all data is publicly accessible.  By creating a small interface for interacting with the Tellor contracts, values from the Tellor oracle can be utilized by the Ampleforth market-oracle.  The AMPL system will simply need to add Tellor to its list of oracles and medianize/average the prices accordingly.  

The integrator pieces and contracts can be found here:  https://github.com/ampleforth/Tellor/pull/1 (AMPL can make this public when they see fit, or we can make it into a PR on the market oracle, up to them)

## Test Cases
In the pull request are various test cases for locally testing the pull request.  To follow best practices, the Tellor team will assist in testing on an Ethereum testnet before launch to mainnet.  Things that can be examined and solidified are the automation of the updates on Tellor and the analysis of the security and liveness of the system.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
