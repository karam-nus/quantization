---
title: "Chapter 24 — GPTQ"
---

[← Back to Table of Contents](./README.md)

# Chapter 24 — GPTQ

> *"Quantize one column, then move the error into the columns you have not quantized yet."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV. This is the book's cleanest derivation — do it in full.

## Overview

## The Lineage

### Optimal Brain Damage

### Optimal Brain Surgeon

### OBC — Optimal Brain Compression

### GPTQ

## The Derivation

### The layer-wise objective

### The Hessian $H = 2XX^\top$ from calibration activations

### Closed-form update after quantizing one column

### The Cholesky reformulation and why it is numerically stable

### Lazy batch updates

### Why an arbitrary column order works

## The Knobs

### `group_size`

### `desc_act` / act-order, and its kernel cost

### `damp_percent`

### Symmetric vs asymmetric

### Bit-width

## Applying It

### GPTQModel / AutoGPTQ

### llm-compressor

## Failure Modes

### Calibration-domain sensitivity

### Hessian singularity on dead channels

### Degradation below 3-bit without small groups

## Measured Quality

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 25 — AWQ](./25_awq.md) — the same problem attacked without a Hessian.
