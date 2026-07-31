---
title: "Chapter 22 — Why LLMs Are a Different Problem"
---

[← Back to Table of Contents](./README.md)

# Chapter 22 — Why LLMs Are a Different Problem

> *"At batch size one, a decode step is a memory transfer with a small amount of arithmetic attached."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## The Roofline Argument

### Arithmetic intensity of prefill

### Arithmetic intensity of decode at batch 1

### Why weight-only 4-bit gives near-linear decode speedup and nothing for prefill

## Outlier Channels

### Pointer to Chapter 15; what is specific to LLMs

## Per-Token Dynamic Quantization Is Cheap Here

## The KV-Cache Grows With Context

### When it overtakes the weights

## Why "Quantize Weights, Leave Activations Alone" Is Right for LLMs and Wrong for CNNs

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 23 — Weight-Only Baselines](./23_weight_only_baselines.md) — the options that need no calibration at all.
