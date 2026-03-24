---
type: journal
status: active
tags:
  - journal
  - pplns
---

# Implementation Journal

Use this note to capture one breadcrumb per session for the [[PPLNS]].

## Prompts

When you sit down to work, try to write:

- one thing I understand better now
- one thing that is still fuzzy
- one smallest next step

## 2026-03-20

### What I learned

- PPLNS removes the idea of a round and pays over a trailing window instead.
- A naive fixed-share window is conceptually simple, but unit-PPLNS is the paper's fully hopping-proof version.

### What is still fuzzy

- What the most natural Rust API should be for eligible-share calculation.
- Whether the crate should expose pending reward directly or leave it to callers.

### Smallest next step

- Sketch `eligible_slice` on paper using the terms in [[PPLNS Glossary]].

## 2026-03-20 Architecture pass

### What I learned

- The crate should be documented as a deterministic accounting kernel rather than as a pool feature.
- Rosenfeld and Bonazzi or Merli fit together as two layers: base window accounting and optional fee-aware extension.
- Obsidian can carry Mermaid diagrams, which makes architecture notes much easier to read.

### What is still fuzzy

- Whether the core should expose traits or enums first for policies.
- How partial-slice inclusion should behave if a work window cuts a slice boundary.

### Smallest next step

- Refine the core Rust types from [[Payout Crate Architecture]] into a draft public API note.

## 2026-03-20 Visual pass

### What I learned

- The Delving Bitcoin visuals are useful because they make three separate ideas visible: reward decomposition, slice grouping, and `delta`-triggered transitions.
- Mermaid is good enough to recreate those ideas directly in the vault.

### What is still fuzzy

- Whether we eventually want a more quantitative chart for MMEF growth than the current flow-style diagram.

### Smallest next step

- Revisit [[SLICE Visuals]] once the public API draft exists and align the diagrams with the actual type names.

Related notes: [[PPLNS]] and [[PPLNS Rust Crate]]
