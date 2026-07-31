---
title: "Chapter 42 — Hands-On: Quantizing a CNN"
---

[← Back to Table of Contents](./README.md)

# Chapter 42 — Hands-On: Quantizing a CNN

> *"Watch MobileNetV2 collapse under per-tensor weights and recover under per-channel. Everything in Part I is in that one experiment."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII. Long listings go in `<details class="code-fold">` panes.

## Overview

## Setup

### Models: ResNet-18 and MobileNetV2

### Evaluation set and baseline numbers

## Building the Calibrators From Scratch

### Min-max observer

### Percentile observer

### KL-divergence observer

### Verifying each against the theory in Chapter 6

## Quantizing the Weights

### Per-tensor

### Per-channel

## Measuring

### Top-1 accuracy

### Per-layer SQNR

## The Central Experiment

### MobileNetV2 fails with per-tensor weights

### Per-channel rescues it

### Why, traced to a specific depthwise layer

## Results Table

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 43 — Hands-On: Export & Deploy](./43_handson_export_and_deploy.md) — getting it out of PyTorch and proving it stayed integer.
