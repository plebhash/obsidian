---
type: paper
status: active
tags:
  - paper
  - pplns
source: https://arxiv.org/pdf/1112.4980
author: Meni Rosenfeld
year: 2011
---

# Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems

This is the canonical reference for the [[PPLNS]].

## Why this paper matters for the crate

It gives us a first-principles view of pooled mining reward systems:

- what problem pooled mining solves
- why some reward systems are unfair or gameable
- why PPLNS exists
- what tradeoff PPLNS makes between variance and maturity time
- why a naive fixed-share window is not enough once difficulty or reward can change

## Reading order for this project

Recommended order:

1. `1.3` pooled mining
2. `3.3` PPLNS
3. `5.3` PPLNS variants
4. `6.1` pool-hopping
5. `6.2` block withholding
6. `5.2` general unit-based framework, if we want a more general abstraction later

## Core problem statement

Solo mining has the right expected reward but very high variance.

Pools try to preserve the same long-run expected reward while smoothing payouts. The hard part is doing that without creating new exploits or forcing the operator to absorb too much risk.

## Simple PPLNS in paper language

The simple version of PPLNS removes the idea of a round.

Instead, when a block is found, the pool pays the last `N` shares. A simple fixed-`N` version can be thought of like this:

- pick a fee `f`
- pick a window size `N`
- when a block is found, pay each of the last `N` shares `(1 - f) * B / N`

Under the paper's constant-difficulty and constant-reward assumptions:

- expected payout per share is `(1 - f) * p * B`
- reward variance per share is approximately `p * B^2 / N`
- maturity time is `pN / 2`
- variance times maturity time stays fixed at `1/2 * (pB)^2`

Interpretation:

- larger `N` means lower variance
- larger `N` also means slower reward realization
- there is no free lunch; the tradeoff just moves around

## Why simple PPLNS is not enough

The paper warns that simple fixed-`N` PPLNS is not fully hopping-proof if difficulty or block reward can change.

The reason is subtle but important:

- the miner's contribution is measured when the share is submitted
- the share's eventual reward depends on future conditions

If future conditions are predictable enough, a hopper can bias when to join or leave.

## Unit-PPLNS

The paper's fully hopping-proof version is unit-PPLNS.

The key idea is to measure the window in units tied to inverse difficulty rather than raw share count.

The implementation-oriented summary is:

1. Choose fee `f` and window size `X`.
2. For each submitted share, store:
   - `units = p_at_submission`
   - `amplifier = B_at_submission`
3. When a block is found, reward earlier shares according to how much of the next `X` units they still cover.

Important details:

- the winning share does not reward itself from its own block
- only shares still inside the `X`-unit window are eligible
- the paper suggests tracking a cumulative total of submitted units so eligibility can be found efficiently

Under this model:

- expected payout per share remains `(1 - f) * p * B`
- variance is approximately `(pB)^2 / X`
- maturity time is `X / 2`
- the same variance vs. maturity tradeoff still exists

The paper also gives a very useful pending-reward expression for a share with amplifier `a`, units `u`, and `U` later units already submitted:

- pending reward = `(1 - f) * u * a * max(X - U, 0) / X`

That formula is a strong candidate for early tests because it turns state into something directly observable.

## What to carry into Rust

For a first crate, I would treat these as the core semantics:

- A share has contribution measured at submission time.
- Payout eligibility is based on a trailing window.
- The window should be expressible in difficulty-aware units.
- Pending reward should be derivable from current state.
- Parameter changes are dangerous and should be explicit in the API.

## First-pass non-goals

These are in the paper, but they do not need to be in version `0.1.0`:

- geometric method
- DGM
- PPS reserve math
- exotic decay functions
- market mechanisms around shares

## Threats to keep in mind

Relevant attack surfaces from the paper:

- proportional pools are vulnerable to pool-hopping
- reward schemes should avoid dependence on round age
- block withholding exists and affects pool economics even if the payout math is correct

For the crate, that means:

- fairness proofs matter
- invariants matter
- bookkeeping should be testable with deterministic scenarios

## Open implementation questions

- Do we want to model simple fixed-`N` PPLNS first and unit-PPLNS second?
- Should the crate expose exact payout events, pending balances, or both?
- Do we want integer arithmetic only, or a generic numeric layer?
- How much history do we want the state machine to retain?

Related notes: [[PPLNS]], [[PPLNS Glossary]], and [[PPLNS Rust Crate]]
