---
rfc: 3
title: Remove Pause Functions from Token and Policy
author: Ampleforth Dev Council - Brandon Iles <brandon@ampleforth.org>, Nithin Ottilingam <nithin@ampleforth.org>, Ahmed Naguib Aly <naguib@ampleforth.org>
type: Core
status: Draft
created: 2020-07-24
deployed: N/A
---

## Simple Summary

Proposal to remove the pause functions in the AMPL ERC-20 and Supply Policy contracts.

## Abstract
There are currently two admin-callable functions, *setRebasePaused* and *setTokenPaused*, that were included in the initial
deployment as emergency controls. They were meant to:
- Facilitate the launch itself
- Guard balances in critical situations post-launch

As stated in the pre-launch [governance post](https://www.ampltalk.org/app/forum/governance-20/topic/state-of-discretion-and-governance-in-ampleforth-6/):
> We view both of these emergency switches as options of last resort and hope they never have to be enabled. We felt it was most responsible to launch with these in place, especially in the early days when the system is new and proving its worth in an adversarial environment.

It has now been over a year. After unpausing the system at the time of launch with Bitfinex, there has been no need to enable either of these.
Now that the implementation of the AMPL token and the Supply Policy have had time to prove themselves, we believe it's time they are removed.

## Motivation
**setRebasePaused** fixes the supply of the token until rebase is unpaused--the supply stays constant and 
Amples are still freely tradable by anyone. This was to guard against unexpected problems in the supply policy or oracle system upstream 
of the token.

**setTokenPaused** pauses all transfers on chain. This was meant to guard balances against 
unexpected problems in the token itself. It was also a guard used to help facilitate the launch before listing.

Removing these two functions decreases both "Centralization Risk" and "Integration Risk".

## Rationale
As long as these functions exist, they are open to misuse by whoever controls the admin key (currently a 2-of-n multisig wallet owned by the development team).

Even as the Ampleforth governance process decentralizes, we'd like to keep the governance surface area as small as possible to 
avoid complexity and potential for exploits.

Secondly, Pausability increases the risk for outside platforms which build with AMPL. Not every platform is willing (or able) to implement logic that safely
handles failed transfers of paused tokens. This may harm AMPL's ability to support the wider DeFi ecosystem.

## Implementation
The OpenZeppelin AdminUpgradabilityProxy will be used to update the implementation of both UFragments and UFragmentsPolicy.
The two pause functions would be removed.
Special care will be taken to ensure no changes are made to the storage layout.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
