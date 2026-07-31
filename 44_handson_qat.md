---
title: "Chapter 44 — Hands-On: QAT From Scratch"
---

[← Back to Table of Contents](./README.md)

# Chapter 44 — Hands-On: QAT From Scratch

> *"Write the straight-through estimator yourself once and it stops being magic."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII.

## Overview

## An STE Fake-Quant Autograd Function

### Forward, backward, and the gradient clipping at the range boundary

## An LSQ Quantizer

### The learned step size

### The gradient scale factor, implemented

## Training

### Initializing from the Chapter 42 PTQ solution

### Schedule and observer freezing

## Comparing Against the PTQ Baseline

## Swapping in the Library Implementations

### `torch.ao` QAT modules

### TorchAO

### Confirming agreement

## Results Table

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 45 — Hands-On: Quantizing an LLM](./45_handson_llm.md) — the same discipline applied to a transformer.
