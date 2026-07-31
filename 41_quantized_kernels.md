---
title: "Chapter 41 — How a Quantized Matmul Runs"
---

[← Back to Table of Contents](./README.md)

# Chapter 41 — How a Quantized Matmul Runs

> *"W4A16 does not do 4-bit math. It does 16-bit math on weights that arrived cheaply."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VI.

## Overview

## Integer Tensor-Core Paths

### `dp4a`

### IMMA and the integer MMA instructions

## Mixed-Input GEMM

### W4A16 dequantizes into registers before an FP16 MMA

### The win is bandwidth, not arithmetic

### Marlin

### Machete

### CUTLASS mixed-input support

## Dequant-Fused GEMV for Batch-1 Decode

## Arithmetic Intensity, Computed

### The crossover batch size at which W4A16 stops helping

## The Schemes That Actually Reduce FLOPs

### W8A8

### W4A4

### What they cost in accuracy to get there

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 42 — Hands-On: Quantizing a CNN](./42_handson_cnn_ptq.md) — Part VII, where all of this gets run.
