---
type: project
status: active
tags:
  - sv2
  - jd
  - mining
---

# JD

This note is the project hub for ideas around how [[SRI Production Pool]] will do Job Declaration.

## Parent context

- [[SRI Production Pool]]
- [[SRI Contributor Work]]

## Why this matters

Job Declaration is one of the key selling points of Stratum V2.

For the production pool effort, JD is not just a protocol checkbox. It is part of the larger question:

How should SRI Production Pool support miner-selected templates in a way that is practical, compelling, and aligned with the goal of more decentralized bitcoin mining?

## Goal

Organize ideas about how SRI Production Pool should approach Job Declaration at a high level without losing sight of the fact that SRI JD is already fairly mature.

## Scope

This project should hold thoughts about:

- why JD matters for SRI Production Pool
- what a JD-enabled pool mode should look like
- what product, protocol, and operational constraints matter
- what dependencies JD creates for payout and accounting

## Current design constraint

The Sv2 JD spec standardizes one designated pool payout output even though the coinbase may contain additional outputs.

That matters because a future non-custodial community payout story cannot be hand-waved as "just pay many recipients directly from the pool coinbase layout."

Current working conclusion:

- native non-custodial community payout support inside the JD pooled-mining flow will require an Sv2 protocol extension
- otherwise SRI will need some settlement mechanism outside the immediate JD payout rail

See [[JD Payout Constraints]].

## Relationship to other projects

### [[PPLNS]]

PPLNS is a parallel project, not a child project of JD.

If SRI Production Pool eventually supports pooled JD, then template-aware payout and accounting become part of the problem.

So the relationship is:

- JD and PPLNS can advance in parallel
- PPLNS is a likely prerequisite for some future pooled-JD payout paths
- JD is broader than payout alone

### [[OpenClaw]]

OpenClaw is adjacent but separate.

It is about operations and agent assistance, not the protocol or payout mechanics of JD itself.

## Visual map

```mermaid
flowchart TD
    A["JD"] --> B["Pool behavior and product shape"]
    A --> C["Protocol and product questions"]
    A --> E["Operational and rollout questions"]
    D["PPLNS"] -. "payout prerequisite" .-> A
```

## Current questions

- how SRI Production Pool should present and operationalize JD now that the core JD story is already fairly mature
- where JD support stops being "solo pool plus extras" and becomes a pooled-mining system
- which parts of the JD story belong in the pool itself versus surrounding modules
- what can remain future-facing without blocking near-term pool goals

## What belongs here

- high-level architecture thoughts
- feature framing
- relationship to payout design
- protocol and product questions

## What does not belong here

- low-level payout formulas
- detailed Rust API sketches
- operations automation details

Those belong in more specific child or sibling notes.

## Next smallest step

Create the first JD design note describing what "mature JD on SRI Production Pool" should mean in product, protocol, and rollout terms.

Related notes: [[SRI Production Pool]], [[PPLNS]], and [[OpenClaw]]
