---
title: "Chapter 29 — FP8 & FP4"
---

[← Back to Table of Contents](./README.md)

# Chapter 29 — FP8 & FP4

> *"Floating point at low bit-widths keeps dynamic range and spends its precision where the data is dense."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## FP8 Recipes

### Per-tensor static

### Per-tensor dynamic

### Per-token and per-channel

### Delayed scaling and amax history in training

### E4M3 forward, E5M2 backward — why

## Transformer Engine

## FP4

### NVFP4: block of 16, FP8 E4M3 scale, plus a per-tensor FP32 scale

### MXFP4: block of 32, E8M0 power-of-two scale

### Direct comparison — the block size and scale format are the whole story

### Blackwell's native FP4 path

## What Is Known About FP4 Quality

### And where the honest uncertainty is

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 30 — 2-Bit and Below](./30_extreme_low_bit.md) — past the point where uniform grids survive.
