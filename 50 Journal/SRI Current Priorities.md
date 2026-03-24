---
type: journal
status: active
tags:
  - sri
  - release
  - coordination
---

# SRI Current Priorities

This note tracks near-term things plebhash needs to keep moving on SRI work.

## Current release cycle

- We are in the middle of planning patch release `0.3.1`.

## Ongoing work

### 1. `sv2-apps`

- Ongoing work is happening around the `v31rc0` branch.

### 2. `sv2-ui`

- The `sv2-ui` release is part of the current coordination picture.
- Relevant issue: [sv2-ui #31](https://github.com/stratum-mining/sv2-ui/issues/31#issue-4126491176)
- Current takeaway from gitgab19 discord chat context:
  - after the next `sv2-apps` patch release, `sv2-ui` should use stable Docker image tags
  - if this lands after Thursday, plebhash may need to do it directly
  - the release should come from a `release/v0.1.0` branch, similar to `sv2-apps`

## Current checklist

- [ ] Keep patch release `0.3.1` planning visible and explicit.
- [ ] Keep track of the `v31rc0` branch work in `sv2-apps`.
- [ ] Follow through on the `sv2-ui` release dependency on stable Docker image tags after the `sv2-apps` patch release.

## Open coordination questions

- What is definitely in scope for `0.3.1`?
- What still belongs only to `v31rc0` and not to the patch release?
- What concrete action will plebhash need to take for the `sv2-ui` release path?

## Next smallest step

- Turn the current checklist into a release-oriented action list once the `0.3.1` scope is clearer.

Related notes: [[SRI Contributor Work]] and [[SRI Production Pool]]
