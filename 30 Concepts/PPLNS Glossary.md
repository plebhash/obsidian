---
type: glossary
status: active
tags:
  - pplns
  - concepts
---

# PPLNS Glossary

## Block reward `B`

The reward attached to a found block in the paper's formulas.

For a clean crate design, treat this as input data, not a global constant.

## Difficulty `D`

The network difficulty.

The paper often reasons in terms of inverse difficulty when expressing expected contribution.

## Inverse difficulty `p`

The paper uses `p` as the contribution-sized unit associated with a share.

For our purposes:

- larger difficulty means smaller `p`
- `p` is what lets unit-PPLNS stay fair across difficulty changes

## Share

A submitted proof-of-work sample that is valid at pool difficulty.

In the crate, a share will probably need:

- who submitted it
- when or in what order it arrived
- its contribution units
- the reward amplifier in effect at submission time

## Winning share

The share that actually solves a block.

In the paper's unit-PPLNS description, the winning share does not receive payout from its own block event.

## Window

The trailing region of recent work that is still eligible for payment.

Two common ways to describe it:

- raw shares: fixed `N`
- contribution units: fixed `X`

## Slice

In SLICE, a slice is a local comparison group inside the larger payout window.

Important:

- a slice is not the same thing as the full PPLNS window
- slices exist so fee value is compared only among shares submitted under roughly comparable conditions

## Job Declaration

A Stratum V2 capability that lets miners declare and mine their own block templates.

This matters because payout can no longer assume all miners are contributing toward the same fee opportunity.

## Template value

The revenue-relevant value attached to the job or template a miner is working on.

In the Bonazzi or Merli paper this is modeled in terms of fees. For crate design, it may be useful to think in the slightly broader term "template value" and let the caller decide how that value is produced.

## Reward components

The distinct budgets that may be distributed for one reward event.

The most important current components are:

- subsidy
- fees

## Entitlement

The amount or weight a participant has earned according to the accounting rules.

This is intentionally more general than "payment", because the crate should compute accounting results without deciding settlement behavior.

## Pool-hopping

A strategy where miners try to join only when the current reward system gives them above-fair expected value.

One of the main goals of PPLNS is to remove that edge.

## Variance

How noisy payouts are around their expected value.

Lower variance feels better for miners, but usually comes with slower reward realization or more operator risk.

## Maturity time

How long, on average, it takes for a share's reward to become realized.

In PPLNS, reducing variance tends to increase maturity time.

## Pending reward

Reward that is not fully realized yet but is expected from current shares under the current state.

This is a useful derived value for both tests and UI.

## Fairness

At a minimum, a fair hopping-resistant method should preserve the expected payout per share.

In this vault, "fair" means we should always be able to explain why a share's expected value is proportional to its contribution, and when relevant, to the template value attached to that contribution.

Related notes: [[PPLNS]], [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]], [[Bonazzi Merli 2024 - PPLNS with Job Declaration]], and [[PPLNS Rust Crate]]
