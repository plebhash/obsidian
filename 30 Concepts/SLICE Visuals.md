---
type: visual
status: active
tags:
  - slice
  - visual
  - mermaid
source:
  - https://delvingbitcoin.org/t/pplns-with-job-declaration/1099/31?u=plebhash
  - https://delvingbitcoin.org/t/pplns-with-job-declaration/1099/40?u=plebhash
---

# SLICE Visuals

This note collects visual aids for understanding SLICE and recreates the main ideas in Mermaid.

External references:

- [Delving Bitcoin post 31](https://delvingbitcoin.org/t/pplns-with-job-declaration/1099/31?u=plebhash)
- [Delving Bitcoin post 40](https://delvingbitcoin.org/t/pplns-with-job-declaration/1099/40?u=plebhash)

What those posts contribute:

- a reward decomposition view
- a slice grouping view
- an MMEF-growth view with clearer `delta` steps

## Interpretation note

These diagrams are design aids, not normative protocol text.

The canonical semantics still come from:

- [[Rosenfeld 2011 - Analysis of Bitcoin Pooled Mining Reward Systems]]
- [[Bonazzi Merli 2024 - PPLNS with Job Declaration]]

## 1. Reward decomposition

This is the most important visual shift from plain PPLNS to SLICE.

```mermaid
flowchart TD
    A["Found block reward"] --> B["Subsidy component"]
    A --> C["Fee component"]

    B --> D["Work-based weighting over the PPLNS window"]
    C --> E["Slice-local fee-aware weighting"]

    D --> F["Subsidy entitlement"]
    E --> G["Fee entitlement"]

    F --> H["Final entitlement"]
    G --> H
```

## 2. Slices inside a broader PPLNS window

The PPLNS window is the full eligible trailing region.

Slices are smaller local comparison groups inside that region.

```mermaid
flowchart LR
    subgraph W["PPLNS window"]
        subgraph R1["prev_hash = H1"]
            S1["Slice S1<br/>ref fee = f0"]
            S2["Slice S2<br/>ref fee = f1"]
        end

        subgraph R2["prev_hash = H2"]
            S3["Slice S3<br/>new round"]
            S4["Slice S4<br/>ref fee = f3"]
        end
    end
```

Reading rule:

- the window may span multiple rounds
- a slice must not cross a `prev_hash` boundary
- a slice is smaller than the whole window

## 3. When a new slice starts

This is the operational decision rule from Bonazzi and Merli.

```mermaid
flowchart LR
    A["Incoming share"] --> B{"Same prev_hash?"}
    B -- "no" --> C["Start new slice"]
    B -- "yes" --> D{"share fee >= ref fee + delta?"}
    D -- "yes" --> C
    D -- "no" --> E["Keep share in current slice"]
    C --> F["Update reference job and reference fee"]
```

## 4. MMEF intuition with delta steps

The point of slices is that fee opportunity grows through time, so fee comparison should stay local.

```mermaid
flowchart LR
    A["Round starts<br/>reference fee = f0"] --> B["Shares remain in S0<br/>while fee < f0 + delta"]
    B --> C["A share arrives with fee >= f0 + delta"]
    C --> D["Start S1<br/>reference fee = f1"]
    D --> E["Shares remain in S1<br/>while fee < f1 + delta"]
    E --> F["A share arrives with fee >= f1 + delta"]
    F --> G["Start S2<br/>reference fee = f2"]
```

This is the qualitative picture behind the updated MMEF visual from the Delving thread.

## 5. Where each paper fits

```mermaid
flowchart LR
    A["Rosenfeld"] --> B["Trailing window"]
    A --> C["Work fairness"]
    A --> D["Variance vs maturity"]

    E["Bonazzi/Merli"] --> F["Reward split"]
    E --> G["Slices"]
    E --> H["Auditability"]

    B --> I["Payout crate architecture"]
    C --> I
    D --> I
    F --> I
    G --> I
    H --> I
```

## Suggested use in the vault

- use this note when the paper text feels too dense
- link back to it from architecture notes
- refine diagrams as the crate API becomes more concrete

Related notes: [[PPLNS and SLICE Distillation]], [[Payout Crate Architecture]], and [[PPLNS]]
