---
title: "Chapter 25 — AWQ"
---

[← Back to Table of Contents](./README.md)

# Chapter 25 — AWQ

> *"The important weights are not the large ones. They are the ones the large activations touch."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## The Observation

### Protecting ~1% of channels recovers most of the loss

### Salience measured by activation magnitude, not weight magnitude

## The Mechanism

### Per-channel scaling as a diagonal similarity transform

### Why the float output is unchanged

### The grid search over the scaling exponent

## No Backward Pass, No Hessian

### Why AWQ is much faster to apply than GPTQ

## Kernels

### GEMM version

### GEMV version

## Applying It

### AutoAWQ

### llm-compressor

## Failure Modes and Where AWQ and GPTQ Disagree

## Measured Quality

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 26 — SmoothQuant](./26_smoothquant.md) — the same scaling idea, aimed at activations instead of weights.
