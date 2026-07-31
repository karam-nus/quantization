---
title: "Chapter 38 — PyTorch Quantization"
---

[← Back to Table of Contents](./README.md)

# Chapter 38 — PyTorch Quantization

> *"Three generations of API, two of them still in the docs, one of them the answer."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VI.

## Overview

## The Three Generations, Honestly Labeled

### Eager mode — manual, legacy

### FX graph mode — maintenance

### PT2E — `torch.export` plus a backend `Quantizer`, the current path

## Observers

### Mapping the qscheme options onto Chapter 6's calibrators

### `QConfigMapping`

### Writing a custom observer

## QAT Modules

## TorchAO

### The `quantize_` API

### Affine-quantized-tensor subclasses

### Available weight-only and dynamic schemes

## What Each Path Can and Cannot Express

## Export Targets

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 39 — Vendor & Edge Toolchains](./39_deployment_toolchains.md) — where the model actually has to land.
