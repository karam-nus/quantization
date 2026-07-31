---
title: "Chapter 19 — CNN QAT Recipes"
---

[← Back to Table of Contents](./README.md)

# Chapter 19 — CNN QAT Recipes

> *"QAT is a training run. Budget it like one, and it will pay you back exactly once."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part III.

## Overview

## When to Reach for QAT

## The Recipe

### Epoch budget

### Learning rate, typically 1–10% of the original

### Observer freezing schedule

### BatchNorm freezing schedule

### Order of operations

## Baking In the Deployment Target's Constraints

### Symmetric-only

### Power-of-two scales

### Restricted range

## Case Study: Recovering MobileNetV3 at INT8

## What QAT Cannot Fix

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 20 — Detection & Dense Prediction](./20_detection_and_dense_prediction.md) — where classification intuitions stop holding.
