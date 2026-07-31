---
title: "Chapter 18 — CNN PTQ Recipes"
---

[← Back to Table of Contents](./README.md)

# Chapter 18 — CNN PTQ Recipes

> *"Per-channel symmetric weights, per-tensor asymmetric activations. Start here; earn every deviation."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part III.

## Overview

## The Canonical Configuration

### Why per-channel symmetric weights

### Why per-tensor asymmetric activations

### Why this is the default in every toolchain

## Target-Driven Variations

### TFLite

### TensorRT

### Qualcomm QNN

### ARM Ethos-U

## Calibration for Vision

### Set size and composition

### Preprocessing must match deployment exactly

## Expected Accuracy Deltas by Architecture Family

## The Escalation Ladder

### Better calibrator → per-channel → CLE + bias correction → AdaRound/BRECQ → partial FP fallback → QAT

### Deciding when to stop

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 19 — CNN QAT Recipes](./19_cnn_qat_practice.md) — the last rung of the ladder.
