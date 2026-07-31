---
title: "Chapter 23 — Weight-Only Baselines"
---

[← Back to Table of Contents](./README.md)

# Chapter 23 — Weight-Only Baselines

> *"Establish what round-to-nearest gets you before you believe anyone's claim about beating it."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV. Each method needs the PROMPT §3.2 fact block.

## Overview

## Round-to-Nearest With Grouping: the True Baseline

## LLM.int8()

### The mixed-precision decomposition

### The outlier threshold

### Accurate and slow — why

## bitsandbytes NF4

### NF4's construction

### Double quantization

### Exactly as QLoRA uses it

## HQQ

## optimum-quanto

## TorchAO

### `int4_weight_only`, `int8_weight_only`

### The tinygemm layout

## Comparison: Quality, Speed, and Time to Apply

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 24 — GPTQ](./24_gptq.md) — the first method that must beat these baselines to justify itself.
