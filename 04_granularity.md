---
title: "Chapter 4 — Granularity: Where the Scales Live"
---

[← Back to Table of Contents](./README.md)

# Chapter 4 — Granularity: Where the Scales Live

> *"A per-channel scale is indexed by meaning. A block scale is indexed by memory address. That difference explains most of the format landscape."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part I. This is the home chapter for the granularity thread.

## Overview

## The Granularity Ladder

### Per-tensor

### Per-axis / per-output-channel

### Per-group (along the input dimension)

### Per-block (along linear memory)

### Per-token

### Per-head

### Micro-scaling

## Block-Wise Is Not Channel-Wise

### A channel scale is a semantic index

### A block scale is a memory index, chosen for the dequantizing kernel

### Why GGUF and MX are block-based

### Why GPTQ and AWQ group along the input dimension

## Overhead Accounting

### Effective bits per weight, derived

### Worked examples: INT4 g=128 with FP16 scale + zero; GGUF Q4_K's two-level scheme

### Double quantization

## Activations: Static Per-Tensor vs Dynamic Per-Token

## Granularity × Overhead × Hardware Support

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 5 — Q/DQ in the Graph](./05_graph_representation.md) — how these choices get written down.
