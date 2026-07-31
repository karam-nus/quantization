---
title: "Chapter 32 — Fine-Tuning Quantized Models"
---

[← Back to Table of Contents](./README.md)

# Chapter 32 — Fine-Tuning Quantized Models

> *"Training through a frozen 4-bit base works. Merging the result back into it usually does not."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## QLoRA

### NF4 base, double quantization, paged optimizers

### BF16 LoRA adapters

### The gradient path through a frozen 4-bit base

## LoftQ

### SVD-based initialization to close the quantization gap

## QA-LoRA

### Group-wise adaptation that keeps the merged model quantized

## EfficientQAT

### Block-wise training, then end-to-end

## BitDelta

## The Merge-Time Trap

### Merging a BF16 adapter into an NF4 base and re-quantizing

### What you lose and how to measure it

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 33 — MoE, SSMs & Multimodal](./33_moe_ssm_multimodal.md) — architectures that break the standard recipe.
