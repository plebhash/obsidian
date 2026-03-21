---
type: paper
status: active
tags:
  - paper
  - pplns
  - slice
  - jd
source: https://www.dmnd.work/pplns-with-job-declaration/pplns-with-job-declaration.pdf
authors:
  - Lorenzo Bonazzi
  - Filippo Merli
year: 2024
companion: https://blog.dmnd.work/understanding-slice-pplns-jd/
---

# Bonazzi Merli 2024 - PPLNS with Job Declaration

This is the second canonical paper for the [[PPLNS Project]].

It answers a question Rosenfeld did not need to answer in 2011:

How should a pool pay miners fairly when miners are allowed to build different block templates?

## Why this paper matters for the crate

This paper gives the bridge from classic PPLNS to decentralized template selection:

- block subsidy and block fees are not treated the same
- work alone is not enough to price contribution once miners choose templates
- fee comparison must be local in time, not across an entire window
- auditability matters because the operator could otherwise fake or dilute shares

## Reading order for this project

Recommended order:

1. `1` introduction
2. `2` slices
3. `3.1` difficulty-based score
4. `3.2` fee-based score
5. `3.3` payout for each share
6. `4` shares accountability
7. `Appendix A` motivation for slices

## Core problem statement

Traditional PPLNS assumes miners are effectively working on the same block template.

That stops being fair under Job Declaration. Two miners can contribute the same amount of work while aiming at templates with different fee value. If payout ignores that difference, template selection becomes economically invisible, which is the wrong incentive in a decentralized setting.

## Main idea

The paper proposes splitting payout into two components:

- subsidy payout, distributed by work contribution
- fee payout, distributed by work contribution and template fee value

This is the important architectural takeaway:

PPLNS remains the base accounting mechanism, but fee payout becomes an extra policy layer.

## Slices

A slice is a local comparison group inside the broader PPLNS window.

The paper's motivation is that mempool extractable value grows within a mining round. If we compare fee value across a long period, earlier shares are unfairly penalized just because they were submitted before higher-fee transactions arrived.

So a slice:

- must not cross a `prev_hash` boundary
- should be short enough that fees are approximately comparable inside it
- is created by the pool using a reference job with fee value `f`

A new slice begins when:

1. a share arrives with fees `>= f + delta`
2. a share arrives with a different `prev_hash`

## Scores

The paper defines two scores.

### Difficulty-based score

For a share `s_i` in the active window:

- `score_d(s_i) = d_i / sum(d_j over the window)`

This is the work-based share of the payout.

### Fee-based score

For a share `s_i` inside a particular slice:

- `score_f(s_i) = score_d(s_i) * f_i / sum(score_d(s_j) * f_j over the slice)`

This keeps fee payout proportional to both:

- contributed work
- template fee value

## Payout formula

If:

- `r` is the block subsidy
- `F` is the block fee
- the window contains `t` slices
- each slice gets `F_S = F / t`

then the payout of a share `s_i` in slice `S` is:

- `payout(s_i) = r * score_d(s_i) + F_S * score_f(s_i)`

The key design lesson is that one block reward is decomposed into components, each with its own weighting rule.

## Why slices exist

The appendix is especially important for implementation intuition.

If a slice is as large as a whole mining round, then later shares in the round can become systematically more valuable as mempool value rises. That reintroduces a hopping-like incentive. Slices are the paper's answer to that.

## Auditability requirements

The paper expects enough published data for miners to challenge the operator.

For each relevant slice, the operator should be able to expose things like:

- number of shares
- sum of share difficulties
- fee value of the reference job
- Merkle root of the slice's shares
- reference job id

That means auditability is not an afterthought. It is part of the payout design.

## What to carry into Rust

For this crate, the paper suggests:

- reward components should be explicit
- template value should be modeled as share metadata
- slice formation should be a policy
- fee scoring should be local to a slice, not global to the whole window
- audit artifacts should be possible without rewriting the accounting core

## Where this extends Rosenfeld

Rosenfeld gives us:

- trailing-window thinking
- fairness against pool-hopping
- variance and maturity intuition

Bonazzi and Merli add:

- reward-component decomposition
- local fee comparability through slices
- a bridge to miner-selected templates
- explicit auditability hooks

## Current interpretation

My current reading is:

- Rosenfeld tells us what the base windowed accounting should look like
- Bonazzi and Merli tell us how to adapt payout calculation when template value differs across miners

That suggests a crate architecture with a core accounting layer and an optional SLICE or JD extension layer.

Related notes: [[PPLNS Project]], [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]], [[PPLNS and SLICE Distillation]], [[SLICE Visuals]], and [[Payout Crate Architecture]]
