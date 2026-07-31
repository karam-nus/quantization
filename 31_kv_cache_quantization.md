---
title: "Chapter 31 — KV-Cache Quantization"
---

[← Back to Table of Contents](./README.md)

# Chapter 31 — KV-Cache Quantization

> *"Keys and values are stored together and behave nothing alike."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## KV Memory Arithmetic

### Bytes per token as a function of layers, heads, head-dim, and dtype

### When KV overtakes the weights

## Keys and Values Want Different Granularities

### Key outliers are per-channel

### Value outliers are not

### The evidence from KIVI and KVQuant

## Per-Channel K, Per-Token V

## The Residual Full-Precision Window

## FP8 KV as the Cheap Default

## Interaction With PagedAttention and Prefix Caching

## Measured Quality vs Context Length

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 32 — Fine-Tuning Quantized Models](./32_quantized_finetuning.md) — training on top of a compressed base.
