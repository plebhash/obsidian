---
type: project
status: active
tags:
  - sv2
  - sri
  - pool
source: /Users/plebhash/sri_production_pool.md
---

# SRI Production Pool

This note captures the high-level project around running a production-grade Sv2 pool effort.

Source note: `/Users/plebhash/sri_production_pool.md`

## Current framing

From the slide deck, the project is framed around:

- finding real mainnet blocks with Sv2
- operating a mainnet SRI pool in a real-world setting
- proving reliability and usefulness through production usage

At the moment, the slides frame this mostly as a mainnet solo-pool effort.

## Why this matters

This project feels like the bridge between:

- protocol work
- production operations
- future pooled-mining features

It is also the note that explains why some subprojects exist even if they are not immediate priorities.

## Subprojects

### 1. [[JD]]

This is the project lane for thinking through what Job Declaration should mean for SRI Production Pool.

It exists because JD is one of Sv2's defining ideas, and because a serious JD story for the production pool touches protocol behavior, product shape, and payout design all at once.

This lane is already relatively mature. The open work here is not "should JD exist?" but how SRI Production Pool should shape and operationalize it.

### 2. [[PPLNS]]

This is the future-facing payout and share-accounting lane that supports the pool's longer-term payout story.

The slides explicitly say that JD-style pooled mining would require share accounting and non-custodial payouts, and that this is a non-trivial scope and not the immediate priority.

That makes the PPLNS crate a valid production-pool subproject, even if it is not the first thing to ship.

PPLNS is best thought of as a prerequisite or enabling lane for some JD-related pooled-mining directions, not as a child project of JD itself.

### 3. [[OpenClaw]]

This is the operations and AI-agent lane.

The slides frame it as part of the support system around maintaining a mainnet VPS SRI pool, reporting on crashes, and producing analysis or summaries.

## Visual map

```mermaid
flowchart TD
    A["SRI Production Pool"] --> B["Mainnet solo pool today"]
    A --> C["Future pooled or JD direction"]
    A --> D["JD"]
    A --> E["PPLNS"]
    A --> F["Ops and maintenance tooling"]
    F --> G["OpenClaw"]
    E -. "payout prerequisite" .-> D
```

## Current interpretation

My current reading is:

- the production pool is the practical operational effort
- JD is a mature feature and design lane for miner-selected-template support
- PPLNS is a parallel payout and accounting lane that can advance alongside JD
- PPLNS is a likely prerequisite for some future JD-enabled pooled-mining paths
- OpenClaw is an operational support capability under that umbrella

## What belongs here

- project intent
- project boundaries
- relationship between subprojects
- high-level decisions and priorities

## What does not belong here

- deep payout math
- detailed API design
- low-level automation prompts

Those should live in the subproject notes.

Related notes: [[SRI Contributor Work]], [[JD]], [[PPLNS]], and [[OpenClaw]]
