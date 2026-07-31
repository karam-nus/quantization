---
title: "Chapter 43 — Hands-On: Export & Deploy"
---

[← Back to Table of Contents](./README.md)

# Chapter 43 — Hands-On: Export & Deploy

> *"'It exported' and 'it runs in INT8' are different claims. This chapter is about proving the second one."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII.

## Overview

## PT2E Quantization

### `torch.export` and the backend quantizer

### Inspecting the exported graph

## Export to ONNX With Q/DQ Nodes

### Verifying the node placement

## ONNX Runtime

### Running it

### Checking the execution provider actually used integer kernels

## TensorRT INT8

### Building the engine

### Reading the verbose layer log

### Confirming the precision each layer ran at

## Proving It

### Netron

### Profiler output

### Comparing against an FP16 build

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 44 — Hands-On: QAT From Scratch](./44_handson_qat.md) — when PTQ leaves accuracy on the table.
