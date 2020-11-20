---
accp: 3
title: "Include support for the kMPL/AMPL Uniswap liquidity pool in the Ampleforth sync() function call fired during rebase"
author: Davoice321 (@davoiceamplesense)
discussions-to: https://github.com/ampleforth/AIPs/issues/20
status: Proposed
created: 2020-11-19
requires (*optional): N/A
---

<!--You can leave these HTML comments in your merged ACCP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new ACCPs. Note that an ACCP number will be assigned by an editor. When opening a pull request to submit your ACCP, please use an abbreviated title in the filename, `accp-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the ACCP.-->
This proposal requests a configuration change that will enable the sync() function call that occurs at Ampleforth rebase to include the incentivized kMPL/AMPL Uniswap v2 token pair.

## Abstract
<!--A short (~200 word) description of the variable change proposed.-->
We are requesting a configuration change so that the sync() function called on rebase includes the incentivized kMPL/AMPL Uniswap v2 pair.
## Motivation
<!--The motivation is critical for ACCPs that want to update variables within Ampleforth. It should clearly explain why the existing variable is not incentive aligned. ACCP submissions without sufficient motivation may be rejected outright.-->
The AmpleSense DAO is an independent, community-powered [organization](https://amplesense.io). Its mission is to expand the ecosystem of products and services (including in the areas of education, lending, borrowing and elastic finance) surrounding the Ampleforth protocol.

One of the DAO's critical initial activities was to distribute its governance token KiloAmple (kMPL) to the community via its "kGeyser" liquidity programs. The DAO has committed to distributing 73% of the token supply via these programs. The first kGeyser program, Zeus was launched in early November and rewards AMPL/ETH Uniswap liquidity providers with kMPL tokens.

The second kGeyser liquidity mining program is called Apollo. The AmpleSense DAO’s two-stage governance process revealed ([vote](https://snapshotpage.b-cdn.net/#/amplesense/proposal/QmboVwWmDt8TJxnuMbjpt39io4z3x3Y3j4TmgvkT5yJAKQ)) that AMPL was the overwhelming choice of the community to be paired with the kMPL token in the Apollo kGeyser.

The kMPL/AMPL Uniswap v2 pair is currently [live](https://info.uniswap.org/pair/0x53b784d0fb88f53c6af76839a7eaec8e95729375) on Uniswap and liquidity providers are receiving kMPL rewards for providing liquidity.

However, kMPL/AMPL trading has not yet begun. To activate trading, a configuration change must be made within the Ampleforth protocol to ensure the kMPL/AMPL pair is incorporated into the sync() function call that is fired upon rebase.

The motivation for this ACCP is to request this critical configuration change, which is required to initiate Uniswap trading on the kMPL/AMPL pair.

-
## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
