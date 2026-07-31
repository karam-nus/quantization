---
title: "Chapter 15 — The Outlier Problem"
---

[← Back to Table of Contents](./README.md)

# Chapter 15 — The Outlier Problem

> *"One channel a hundred times larger than its neighbours sets the scale for all of them. That is the whole difficulty."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II. This is the home chapter for the outlier thread; other chapters link here rather than re-explaining.

## Overview

## The Empirical Phenomenon

### Persistent high-magnitude activation channels

### Massive activations at specific token positions

### Attention sinks

### What the distributions actually look like

## Why Outliers Arise

### LayerNorm normalizes per token, leaving per-channel freedom

### The residual stream accumulates

### Dimensions co-opted as bias or no-op channels

## Why They Are Lethal

### A per-tensor scale is set by the max — the arithmetic

### How many codes the bulk of the distribution actually gets

## Fix Family 1 — Clip

### Percentile and KL calibration

### What is lost

## Fix Family 2 — Isolate

### LLM.int8() mixed decomposition

### SqueezeLLM's dense-and-sparse split

### The cost of a second kernel path

## Fix Family 3 — Equalize

### SmoothQuant, cross-layer equalization, OmniQuant's LET

### Free at runtime when folded; moves difficulty into the weights

## Fix Family 4 — Rotate

### Hadamard and learned rotations

### Provable energy spreading

### The online-transform cost

## CNN Outliers Are a Different Phenomenon

### Spatial and input-dependent rather than channel-consistent

### Why this matters for Chapter 34

## Choosing a Fix

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 16 — Non-Uniform & Codebook Quantization](./16_nonuniform_and_codebook.md) — abandoning the uniform grid entirely.
