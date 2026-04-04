---
type: design
status: active
tags:
  - jd
  - payout
  - constraints
  - sv2
source: https://github.com/stratum-mining/sv2-spec/blob/main/06-Job-Declaration-Protocol.md
---

# JD Payout Constraints

This note captures a key payout-design constraint for any future SRI JD or PPLNS pool work.

## Verified spec takeaway

The Sv2 Job Declaration spec does allow multiple coinbase outputs overall.

But it also standardizes one single designated pool payout output inside `AllocateMiningJobToken.Success.coinbase_tx_outputs`.

Important nuance:

- JDS must reserve a pool payout output
- JDC may add more outputs, including non-zero outputs
- the pool payout itself is standardized as one single output
- if template revenue is diverted into other outputs, the pool may pay proportionally smaller rewards

So the hard constraint is not "coinbase can only have one output".

The real constraint is:

The pool-side payout lane is centered on one designated pool payout output, not on a native multi-recipient pool payout layout.

## Why this matters for SRI

If SRI Production Pool wants to pay the community in a non-custodial way, it likely cannot rely on "just have the pool pay many recipients directly in the JD coinbase layout" as the default answer.

That suggests the non-custodial payout path may need to be handled through some other construction.

## Current working conclusion

If SRI wants native non-custodial community payout support as part of the Sv2 JD pooled-mining flow itself, an Sv2 protocol extension will be required.

This is my current inference from the spec, not an explicit sentence written by the spec authors.

Why:

- the spec designates one pool payout output
- miners may add other outputs, but that does not create a standardized multi-recipient payout rail for the pool
- so first-class non-custodial pool distribution is a protocol-gap question, not just an implementation detail

## Design implication

The future design question becomes:

How should community payout happen if the JD pool payout is standardized around one designated pool payout output?

This pushes us toward alternatives such as:

- an Sv2 protocol extension for native non-custodial payout support
- a second-stage payout mechanism
- some distribution construction outside the immediate pool payout output
- a design that separates template revenue accounting from final community settlement

This note does not choose one yet.

It only records the constraint clearly.

## Current interpretation

My current reading is:

- JD gives miners template freedom
- JD does not natively give the pool a clean multi-recipient payout rail
- a future SRI PPLNS pool will need either a deliberate out-of-band settlement design or an Sv2 protocol extension, not just a naive coinbase split

## Open questions

- what kind of Sv2 protocol extension would best fit SRI's goals if native non-custodial payout is desired
- what non-custodial payout pattern fits best with SRI's goals if settlement happens outside the immediate JD payout rail
- what can happen directly in the mining template versus after block discovery
- how this constraint changes the shape of the future PPLNS pool design

Related notes: [[JD]], [[PPLNS]], and [[SRI Production Pool]]
