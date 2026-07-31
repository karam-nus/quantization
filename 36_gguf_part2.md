---
title: "Chapter 36 — GGUF II: K-Quants, I-Quants & Choosing"
---

[← Back to Table of Contents](./README.md)

# Chapter 36 — GGUF II: K-Quants, I-Quants & Choosing

> *"Q4_K_M is not 4 bits, and the reason it is not is the whole design."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part V. Read `ggml-quants.c` and `src/llama-quant.cpp` — the type mixes must be read from source, not guessed.

## Overview

## K-Quants: Two-Level Quantization

### The superblock of 256

### Sub-blocks of 16 or 32

### Sub-block scales quantized against a superblock FP16 scale

### Q4_K byte by byte: 8 sub-blocks, 6-bit scales and mins, `d` and `dmin`, 144 bytes → 4.5 bits/weight

### The same arithmetic for Q2_K, Q3_K, Q5_K, Q6_K

### Effective bits: the true table, not the nominal one

## I-Quants: Codebooks and Lattices

### IQ1_S, IQ1_M, IQ2_XXS … IQ4_XS

### What the codebook is

### Why they reach lower bits than K-quants at equal quality

### Their CPU cost

## Ternary Types: TQ1_0 and TQ2_0

## The Importance Matrix

### What an imatrix contains: per-channel activation statistics from a calibration run

### `llama-imatrix` producing it

### `llama-quantize` consuming it

### Which types require it

### How the calibration corpus choice affects the result

### This is GGUF's answer to AWQ

## Type Mixes: What `_S`, `_M`, and `_L` Change

### Per-tensor type assignment, read from `llama-quant.cpp`

### Attention vs FFN vs embeddings vs output

## The Selection Table

### RAM budget × model size × CPU-vs-GPU × quality floor → type

### Types that are known-bad choices

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 37 — Every Other Format](./37_other_formats.md) — the same decisions, made differently by everyone else.
