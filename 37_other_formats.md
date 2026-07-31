---
title: "Chapter 37 — Every Other Format"
---

[← Back to Table of Contents](./README.md)

# Chapter 37 — Every Other Format

> *"Channel-wise versus block-wise is not a theoretical preference. It is a format-level decision made by whoever wrote the kernel."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part V.

## Overview

## safetensors + compressed-tensors

### How scales, zero-points, and the quantization config are stored

### The vLLM / llm-compressor schema

## GPTQ On Disk

### The packed INT4 layout

### `quantize_config.json`

## AWQ On Disk

### The layout and the reorder trick

## EXL2 and EXL3

## ONNX QDQ On Disk

## TFLite Flatbuffers

### Per-axis quantization parameters

## TensorRT Engines and Calibration Caches

## MLX

## CoreML

## Qualcomm QNN DLC

## Master Table: Granularity by Format

### Format → scale granularity → zero-point support → metadata location → who can execute it

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 38 — PyTorch Quantization](./38_pytorch_quantization.md) — Part VI, the tools that produce these files.
