---
title: "Chapter 47 — The Debugging Playbook"
---

[← Back to Table of Contents](./README.md)

# Chapter 47 — The Debugging Playbook

> *"Indexed by symptom, because that is the only thing you have when it breaks."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII. Organize by symptom, not by method. Each entry: likely causes ranked, the diagnostic that distinguishes them, the fix.

## Overview

## Symptom — Accuracy Collapses to Chance

### Almost always a scale/zero-point sign or dtype bug, not a quantization problem

## Symptom — Small but Unacceptable Drop

### Sensitivity sweep

### Selective fallback

## Symptom — Works in the Framework, Breaks After Export

### Operator semantics mismatch

### How to bisect across runtimes

## Symptom — Fine on Calibration Data, Bad in Production

### Calibration domain mismatch

## Symptom — Only Some Inputs Break

### Outlier-driven clipping

## Symptom — Quantized but Not Faster

### No integer kernel; reading the profiler

## Tooling

### A per-layer SQNR and cosine-similarity harness

### Activation histogram dumps

### Bisection by progressively quantizing prefixes of the network

### Numerical comparison across runtimes

## Known-Issue Catalogue

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 48 — Evaluating Honestly](./48_evaluating_quantized_models.md) — making sure the thing you fixed is the thing that mattered.
