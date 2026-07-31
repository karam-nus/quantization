---
title: "Chapter 9 — Dynamic vs Static Quantization"
---

[← Back to Table of Contents](./README.md)

# Chapter 9 — Dynamic vs Static Quantization

> *"Computing the range at runtime is free when you are already waiting on memory, and expensive when you are not."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II.

## Overview

## Static Quantization

### The calibration pass and the frozen range

### Why edge deployment almost always wants this

## Dynamic Quantization

### Per-token and per-batch range computation

### The runtime cost of a max-reduction

### Why it is nearly free in memory-bound LLM decode

### Why it is expensive in a latency-critical CNN

## Hybrid Schemes

## What Dynamic Quantization Fixes About Outliers — and What It Does Not

## Runtime Support Matrix

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 10 — Data-Free & Zero-Shot](./10_data_free_quantization.md) — quantizing when you cannot see the data.
