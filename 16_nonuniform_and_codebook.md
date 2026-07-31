---
title: "Chapter 16 — Non-Uniform & Codebook Quantization"
---

[← Back to Table of Contents](./README.md)

# Chapter 16 — Non-Uniform & Codebook Quantization

> *"Uniform grids are fast because dequantization is a multiply. Every codebook method pays for accuracy with a lookup."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II.

## Overview

## When the Uniform Grid Is the Wrong Grid

## K-Means Quantization

### SqueezeLLM's sensitivity-weighted k-means

## Vector and Product Quantization

## AQLM

### Additive multi-codebook representation

### Learned codebooks and the training cost

## QuIP#

### Incoherence processing via randomized Hadamard

### The E8 lattice codebook

## NF4 as a Fixed Non-Uniform Grid

## Log and Power-of-Two Grids for Shift-Only Hardware

## The Shared Cost: Lookup Breaks Fused Dequantization

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 17 — What Breaks in a ConvNet](./17_cnn_quantization_anatomy.md) — Part III begins with the architectures that made these problems visible.
