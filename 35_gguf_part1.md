---
title: "Chapter 35 — GGUF I: Container & Block Quantization"
---

[← Back to Table of Contents](./README.md)

# Chapter 35 — GGUF I: Container & Block Quantization

> *"The scale must sit next to the numbers it scales, because the kernel reads memory in a straight line."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part V. Read `ggml-quants.c`, `ggml-common.h`, and `ggml.h` in `ggerganov/llama.cpp` before writing — do not work from memory. Use `.bitfield` strips for every type.

## Overview

## The GGUF Container

### Header, KV-metadata, tensor info, alignment

### The `general.*` and architecture keys

### Why one file carries tokenizer, hyperparameters, and weights

## Block-Wise Quantization From First Principles

### Why llama.cpp does not use per-channel scales

### The dot-product kernel streams weights and dequantizes into SIMD registers

### The scale must be adjacent in memory

### Block of 32 as a cache-line and register-width compromise

### Contrast with GPTQ's grouping along the input dimension

## The Legacy Block Types

### Q4_0 — 32 weights, one FP16 scale, symmetric

### Q4_1 — adds an FP16 minimum

### Q5_0 and Q5_1 — the fifth bit in a separate 32-bit mask

### Q8_0

### Bytes-per-block arithmetic for each

## A Dequantizer From Scratch

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 36 — GGUF II](./36_gguf_part2.md) — the types people actually ship.
