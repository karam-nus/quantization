---
title: "Chapter 26 — SmoothQuant & Activation-Weight Balancing"
---

[← Back to Table of Contents](./README.md)

# Chapter 26 — SmoothQuant & Activation-Weight Balancing

> *"Weights are easy to quantize and activations are hard, so move the difficulty."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## The Transformation

### $Y = (X \operatorname{diag}(s)^{-1})(\operatorname{diag}(s) W)$

### The per-channel rule $s_j = \max|X_j|^\alpha / \max|W_j|^{1-\alpha}$

### What $\alpha$ means and how to choose it

## Folding the Scale Away

### Absorbing into the preceding LayerNorm

### What happens when there is no LayerNorm to fold into

## W8A8 Kernels and the Actual Speedup

## The Lineage: This Is Cross-Layer Equalization

### Same idea, different era, different architecture

### What changed and what did not

## When $\alpha$ Tuning Is Required

## Measured Quality

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 27 — Rotation & Incoherence](./27_rotation_methods.md) — diagonal transforms generalized to orthogonal ones.
