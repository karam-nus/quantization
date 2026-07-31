---
title: "Chapter 3 — The Quantization Formula, Derived"
---

[← Back to Table of Contents](./README.md)

# Chapter 3 — The Quantization Formula, Derived

> *"Two equations. The rest of the book is about choosing their parameters."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part I. Notation must match PROMPT §3.1 exactly — this chapter defines it for the whole book.

## Overview

## The Affine Map

### Deriving scale and zero-point from a range

### Symmetric as the special case $z = 0$

### Signed vs unsigned, and the restricted range $[-127, 127]$

## Rounding

### Round-half-to-even

### Stochastic rounding, and where it actually helps

### Forward reference: learned rounding

## The Clipping/Rounding Trade-Off

### Widening the range trades clipping error for rounding error

### The U-curve, plotted

## Quantization Error

### SQNR for a uniform quantizer: $\approx 6.02b + 1.76$ dB

### Why that formula breaks for real weight and activation distributions

### Error propagation through one linear layer

## Integer-Only Inference, Worked

### The full pipeline with real numbers

### INT32 accumulation

### Requantization to the next layer's scale

### Numerical check against the float result

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 4 — Granularity](./04_granularity.md) — one scale for how many numbers?
