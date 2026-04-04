---
type: project
status: active
tags:
  - sri
  - umbrella
  - sv2
---

# SRI Contributor Work

This note is the umbrella project for the fronts I am working on as a contributor to the Stratum V2 Reference Implementation.

## Why this note exists

The vault should not treat each effort as isolated.

This note gives a high-level place to organize:

- production-pool work
- protocol-adjacent design efforts
- operational tooling
- future subprojects that do not justify a separate top-level area yet

## Current hierarchy

```mermaid
flowchart TD
    A["SRI Contributor Work"] --> B["SRI Production Pool"]
    B --> C["JD"]
    B --> D["PPLNS"]
    B --> E["OpenClaw"]
    D -. "payout prerequisite" .-> C
```

## Active child projects

- [[SRI Production Pool]]

## Current priorities

For time-sensitive coordination and release work, use:

- [[SRI Current Priorities]]

Current emphasis:

- `sv2-apps` `v0.3.1` is shipped
- `v0.3.2` is now the active release cycle
- the project board is the main coordination surface

## Notes on scope

This is intentionally broad and lightweight.

If another front becomes substantial, it can become another child note under this umbrella.

Related notes: [[Projects]], [[SRI Current Priorities]], [[SRI Production Pool]], [[JD]], [[PPLNS]], and [[OpenClaw]]
