---
title: "Chapter 17 — What Breaks in a ConvNet"
---

[← Back to Table of Contents](./README.md)

# Chapter 17 — What Breaks in a ConvNet

> *"Every quantization horror story in computer vision starts with a depthwise convolution."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part III.

## Overview

## Depthwise Separable Convolutions

### Per-channel weight ranges spanning orders of magnitude

### Why MobileNetV2/V3 and EfficientNet are the canonical hard cases

### The arithmetic of a 9-element channel

## Squeeze-and-Excitation Blocks

## Residual Adds and Concatenations

### Forced scale sharing

## Activation Functions

### ReLU6 vs ReLU vs Swish/Hardswish and what each does to the range

## First-Layer and Last-Layer Policy

## BatchNorm Folding Interacting With Per-Channel Ranges

## Catalogue: Architecture → What Breaks → The Fix

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 18 — CNN PTQ Recipes](./18_cnn_ptq_practice.md) — the configuration that works and the ladder for when it doesn't.
