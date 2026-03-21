---
type: design
status: active
tags:
  - rust
  - pplns
  - design
---

# PPLNS Rust Crate

This note is the design anchor for the crate inside the [[PPLNS Project]].

## Design stance

KISS and first principles:

- make the math legible
- make invariants obvious
- prefer pure functions where possible
- keep storage choices separate from payout semantics

Canonical sources:

- [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]]
- [[Bonazzi Merli 2024 - PPLNS with Job Declaration]]

## Version 0 scope

What `0.x` should do:

- represent submitted shares
- represent a configurable PPLNS window
- split reward into explicit components
- compute which prior shares are eligible when a block is found
- compute payouts for a block event
- expose enough state to calculate pending reward

What `0.x` does not need yet:

- networking
- persistence
- pool server integration
- P2PoolV2 protocol behavior
- optimization for huge histories
- every reward method in the paper

## Suggested implementation order

1. Model the domain types.
2. Implement a plain work-based trailing-window engine.
3. Split reward into subsidy and fee components.
4. Write tiny deterministic examples.
5. Upgrade the model to unit-PPLNS-friendly windows.
6. Add SLICE as an optional fee-allocation layer.
7. Add invariants and property-style tests.

## Architecture note

The current architecture document lives in [[Payout Crate Architecture]].

The current paper distillation lives in [[PPLNS and SLICE Distillation]].

## Candidate domain model

This is intentionally rough:

```rust
struct ShareId(u64);
struct MinerId(String);
struct Work(u128);
struct Amount(u128);

struct Share {
    id: ShareId,
    miner: MinerId,
    work: Work,
    template_value: Option<Amount>,
}

struct RewardComponents {
    subsidy: Amount,
    fees: Amount,
}

struct Window {
    size_in_work: Work,
}
```

That is not a final API. It is just a reminder that we should start from semantics, not clever abstractions.

## Invariants worth protecting

- under constant block reward assumptions, total payout triggered by one block should not exceed the configured distributable reward
- no share outside the active window should be paid
- a share's expected value should track its submission-time contribution
- fee payout should not ignore template value when a fee-aware policy is selected
- the winning share should follow the chosen rule explicitly
- payout logic should not depend on hidden round state if we claim PPLNS

## First pure functions to consider

Pick one of these as the first code artifact:

1. `eligible_slice(shares, current_total_units, window_units) -> Range`
2. `payout_weight(share, units_after_share, block_units, window_units) -> Weight`
3. `pending_reward(shares, current_total_units, window_units, fee) -> Vec<PendingReward>`

My recommendation: start with `eligible_slice`. It is small, central, and easy to test.

Once that works, the next high-value pure function is:

`pending_reward(share_units, share_amplifier, units_after_share, window_units, fee)`

The paper-level target is:

`(1 - f) * u * a * max(X - U, 0) / X`

## Testing ideas

- one share, one block
- multiple shares by one miner
- multiple miners in the same window
- a share just inside the window
- a share just outside the window
- difficulty change between shares
- reward change between shares

## Decision log

### 2026-03-20

- We are using the Rosenfeld paper as the canonical reference.
- We are optimizing for clarity before optimization.
- We likely want to understand simple PPLNS first, then implement unit-PPLNS.
- We now also treat Bonazzi or Merli as a canonical extension for decentralized template-aware payout.
- The crate should be a deterministic accounting module, not a pool implementation.

## Next question

Do we want the crate's public API to start from:

- a state machine that accepts shares and blocks, or
- a pure calculator over an append-only share history?

My bias for learning is to start with the pure calculator, then wrap it later if needed.

Related notes: [[PPLNS Project]], [[PPLNS Glossary]], [[PPLNS and SLICE Distillation]], [[Payout Crate Architecture]], and [[Implementation Journal]]
