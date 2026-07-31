---
title: "Chapter 6 — Calibration: Choosing the Range"
---

[← Back to Table of Contents](./README.md)

# Chapter 6 — Calibration: Choosing the Range

> *"Quantization is one formula. Calibration is where all the choices are."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part I. Each algorithm gets: the objective it optimizes, a from-scratch implementation, cost, when it wins, when it fails.

## Overview

## Min-Max

### The algorithm

### Why one outlier batch destroys it

## Moving-Average / EMA Min-Max

## Percentile

### Choosing 99.9 vs 99.99, and the sensitivity to that choice

## Histogram Methods

### Collecting the histogram

### Bin count and dynamic range growth

## KL-Divergence / Entropy Calibration

### The TensorRT algorithm, derived

### Building the 2048-bin reference histogram

### Sweeping candidate thresholds and quantizing the reference distribution

### Computing $D_{KL}(P\|Q)$ and taking the argmin

### The subtleties: bin merging and zero-bin handling

### From-scratch implementation

## MSE / L2 Grid Search

## Cosine Similarity and SQNR Objectives

## Hessian- and Sensitivity-Weighted Range Selection

## Mean-of-Batch-Max

## Calibration-Set Design

### How many samples, and the evidence for 128–512

### Domain match, and what mismatch actually does

### Sequence length effects for LLMs

### Instruction formatting for instruct-tuned models

### The danger of data that is too clean

### How to detect a bad calibration set

## Choosing a Calibrator: Distribution Shape → Method

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 7 — Choosing the Targets](./07_what_to_quantize.md) — which tensors are worth calibrating in the first place.
