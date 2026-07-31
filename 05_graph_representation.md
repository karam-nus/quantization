---
title: "Chapter 5 — Q/DQ in the Graph"
---

[← Back to Table of Contents](./README.md)

# Chapter 5 — Q/DQ in the Graph

> *"The same quantized model has four different spellings, and each toolchain reads only one of them."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part I.

## Overview

## Fake Quant vs Real Quant

### `FakeQuantize` in the forward pass

### An actually-integer kernel, side by side

## ONNX: QDQ Format vs QOperator Format

### `QuantizeLinear` / `DequantizeLinear` semantics

### The `axis` and `block_size` attributes

### Which runtime prefers which representation

## Observers, Quantizers, and QConfig

## BatchNorm Folding, Algebraically

## Bias in INT32

### Why bias needs no scale of its own: $s_b = s_x s_w$

## Fusion and Where Requantization Lands

## Residual Adds and Concatenations

### Why they force scale sharing

### How each toolchain resolves it

## One Model, Four Representations

### PyTorch PT2E

### ONNX

### TFLite

### TensorRT

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 6 — Calibration](./06_calibration.md) — choosing the ranges these nodes carry.
