---
title: "Appendix D — Hardware Capability Matrix"
---

[← Back to Table of Contents](./README.md)

# Appendix D — Hardware Capability Matrix

> *"What each target can execute natively — sourced from vendor documentation, with the document linked."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Appendices. Every row must cite the vendor doc it came from; mark anything inferred as inferred.

## How to Read This Table

## NVIDIA GPUs

### Turing, Ampere, Ada, Hopper, Blackwell

### Precisions, granularity limits, accumulator widths

## AMD GPUs

## Intel CPUs and GPUs

## Arm CPUs

## Arm Ethos-U NPUs

## Qualcomm Hexagon

## Apple Silicon: GPU, AMX, and the Neural Engine

## Google TPU and Edge TPU

## Renesas DRP-AI

## Hailo

## Other Edge NPUs

## Master Matrix

| Target | INT8 | INT4 | FP8 | FP4 | Per-channel weights | Per-token activations | Accumulator | Toolchain | Source |
|--------|:----:|:----:|:---:|:---:|:-------------------:|:---------------------:|:-----------:|-----------|--------|

## Caveats

### "Supported" in a datasheet vs supported by a shipping kernel

### Where the compiler silently falls back

---

**Next:** [Appendix E — Bibliography](./appendix_e_bibliography.md)
