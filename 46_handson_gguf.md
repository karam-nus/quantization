---
title: "Chapter 46 — Hands-On: GGUF End to End"
---

[← Back to Table of Contents](./README.md)

# Chapter 46 — Hands-On: GGUF End to End

> *"Convert, calibrate, quantize, measure — then open the file and check that the bytes are what Chapter 35 said they would be."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII.

## Overview

## Conversion

### `convert_hf_to_gguf.py`

### Choosing the intermediate precision

## Generating an Importance Matrix

### `llama-imatrix`

### Choosing the calibration corpus

## Quantizing to Several Types

### `llama-quantize` across Q4_K_M, Q5_K_M, Q6_K, IQ4_XS, Q8_0

### With and without the imatrix

## Measuring

### `llama-perplexity`

### `llama-bench`

### File sizes

## The Size-vs-Quality Curve

## Reading a Block Back

### Using the NumPy dequantizer from Chapter 35

### Confirming the layout

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 47 — The Debugging Playbook](./47_debugging_playbook.md) — what to do when any of this goes wrong.
