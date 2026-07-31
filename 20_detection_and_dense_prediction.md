---
title: "Chapter 20 — Detection & Dense Prediction"
---

[← Back to Table of Contents](./README.md)

# Chapter 20 — Detection & Dense Prediction

> *"A classifier only has to rank. A detector has to produce coordinates, and coordinates do not tolerate coarse grids."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part III.

## Overview

## Why Detectors Quantize Worse Than Classifiers

### Regression heads: unbounded outputs, small useful dynamic range

### The score head's tail matters

### NMS and box decode stay in float

## Architecture Specifics

### YOLO family

### SSD

### DETR-style transformer detectors

## Semantic Segmentation

### Boundary-pixel degradation

## Evaluating With mAP and IoU

### Why mAP can collapse while classification-style metrics look fine

### Which IoU thresholds reveal quantization damage first

## Recipes

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 21 — Quantizing Vision Transformers](./21_vision_transformers.md) — the bridge to Part IV.
