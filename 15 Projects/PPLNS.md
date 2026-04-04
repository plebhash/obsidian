---
type: project
status: active
tags:
  - pplns
  - rust
---

# PPLNS

This is the project hub for the PPLNS Rust crate journey.

## Parent context

- [[SRI Production Pool]]
- [[SRI Contributor Work]]

## Goal

Understand PPLNS from first principles and turn that understanding into a small, clear Rust crate.

## Relationship to JD

[[JD]] is closely related, but PPLNS is not a child project of JD.

The better model is:

- both are subprojects of [[SRI Production Pool]]
- PPLNS is an enabling lane for payout and accounting
- some future JD-enabled pool directions may depend on that payout lane
- both can still advance in parallel

## Important system constraint

For future SRI pooled JD work, the payout problem is constrained by the fact that JD standardizes one designated pool payout output rather than a native multi-recipient pool payout layout.

That means a non-custodial community payout story will likely need something more deliberate than a naive direct coinbase split.

Current working conclusion:

- if SRI wants native non-custodial payout support in the JD pooled-mining path, that will require an Sv2 protocol extension
- the PPLNS crate should therefore avoid assuming that protocol-native community settlement already exists

See [[JD Payout Constraints]].

## Canonical references

- [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]]
- [[Bonazzi Merli 2024 - PPLNS with Job Declaration]]

External links:

- [Rosenfeld PDF](https://arxiv.org/pdf/1112.4980)
- [Bonazzi/Merli PDF](https://www.dmnd.work/pplns-with-job-declaration/pplns-with-job-declaration.pdf)
- [Understanding SLICE blog post](https://blog.dmnd.work/understanding-slice-pplns-jd/)

## Core project notes

1. [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]]
2. [[Bonazzi Merli 2024 - PPLNS with Job Declaration]]
3. [[PPLNS and SLICE Distillation]]
4. [[SLICE Visuals]]
5. [[PPLNS Glossary]]
6. [[PPLNS Rust Crate]]
7. [[Payout Crate Architecture]]
8. [[Implementation Journal]]

## Suggested reading route

- `1.3` pooled mining
- `3.3` PPLNS
- `5.3` PPLNS variants
- `6.1` pool-hopping
- `6.2` block withholding

Then switch to the Bonazzi or Merli paper:

- `2` slices
- `3.1` difficulty-based score
- `3.2` fee-based score
- `3.3` payout for each share
- `4` accountability
- `Appendix A` motivation for slices

## Current focus

- keep the crate small and explicit
- favor easy-to-test semantics
- separate core accounting from deployment context
- understand how SLICE extends PPLNS without swallowing the core
- keep this as a production-pool subproject that can progress in parallel with JD

## Next smallest step

Read [[Payout Crate Architecture]] and [[PPLNS and SLICE Distillation]] together.
