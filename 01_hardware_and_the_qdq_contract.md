---
title: "Chapter 1 — Hardware Compatibility & the Q/DQ Contract"
---

[← Back to Table of Contents](./README.md)

# Chapter 1 — Hardware Compatibility & the Q/DQ Contract

> *"A quantized model is a promise about arithmetic. The hardware decides whether the promise is kept."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part I.

## Overview

## What an Accelerator Actually Executes

### GPU tensor-core paths: INT8, INT4, FP8, FP4

### Integer-only DSP and NPU MAC arrays

### CPU SIMD: AVX-512 VNNI and Arm NEON `sdot`

### What "supported" means in each case

## Why Q/DQ Nodes Exist

### Q/DQ as a graph annotation, not an operation

### Lifecycle A — simulated quantization (fake quant) executing in float

### Lifecycle B — Q/DQ folded into a genuinely integer kernel

### Lifecycle C — Q/DQ surviving as real conversion ops at a precision boundary

## Accumulators, Overflow, and Saturation

### Why INT8 inputs need INT32 accumulation

### 16-bit accumulators and the channel-count ceiling

## Requantization Between Layers

### The three realizations of the scale multiply

### FP32 multiply

### Fixed-point integer multiplier and shift (derived)

### Power-of-two shift only

## Granularity Support Is a Hardware Property

### Which targets support per-channel weights, and which do not

### Symmetric-only hardware and what it costs on asymmetric activations

## Do You Need Quantization at All?

### Memory-bound vs compute-bound: the roofline argument

### When INT8 is slower than FP16

## Decision Flowchart: Hardware × Model × Objective

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 2 — Number Formats & Bit Layouts](./02_number_formats.md) — the alphabet the hardware reads.
