---
title: "Chapter 12 — QAT Foundations"
---

[← Back to Table of Contents](./README.md)

# Chapter 12 — QAT Foundations

> *"The gradient of a step function is zero everywhere. QAT works by lying about that, carefully."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II.

## Overview

## The Straight-Through Estimator

### The forward pass, the backward lie

### The bias it introduces

### Why it works anyway

## Fake-Quant Modules

## Learned Quantizers

### LSQ — gradient with respect to the step size, and the gradient scale factor

### LSQ+ — learned offset for asymmetric activations

### PACT — learned clipping on activations

### DSQ — differentiable soft quantization

### TQT — power-of-two constrained thresholds

## BatchNorm During QAT

### Freezing statistics

### The folding-order problem

## Initialization From a PTQ Solution

## Recipes That Converge

### Learning rate, schedule, and epoch budget

### What to freeze and when

## Cost Model

## Failure Modes

### Weight oscillation around a decision boundary

### Dead codes

### BN statistic drift

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 13 — Quantization-Aware Distillation](./13_quantization_aware_distillation.md) — adding a teacher to the QAT loop.
