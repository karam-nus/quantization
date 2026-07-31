---
title: "Chapter 14 — Mixed Precision & Bit Allocation"
---

[← Back to Table of Contents](./README.md)

# Chapter 14 — Mixed Precision & Bit Allocation

> *"A uniform bit-width is a statement that every layer is equally important. No network has ever satisfied it."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II.

## Overview

## Sensitivity Analysis, the Practical Version

### Quantize one layer at a time and measure

### Cost and what it misses

## HAWQ

### v1 — Hessian top eigenvalue as a sensitivity proxy

### v2 — Hessian trace and relative sensitivity

### v3 — the integer linear programming formulation

## HAQ — Reinforcement Learning Search

## Building a Pareto Frontier

## How Shipped Systems Do It

### EXL2's per-layer bit search

### GGUF's per-tensor type mixes

### Keeping `lm_head` and the first block higher

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 15 — The Outlier Problem](./15_outliers.md) — the phenomenon that most of Part IV is organized around.
