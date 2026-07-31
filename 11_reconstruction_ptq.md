---
title: "Chapter 11 — Layer-wise Reconstruction PTQ"
---

[← Back to Table of Contents](./README.md)

# Chapter 11 — Layer-wise Reconstruction PTQ

> *"Rounding to nearest is optimal for one weight and provably suboptimal for a layer."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II. This chapter is what makes Chapter 24 easy.

## Overview

## The Reconstruction Objective

### $\|WX - \hat{W}X\|^2$ and why it beats element-wise rounding

### The data requirement: unlabeled activations only

## AdaRound

### Rounding as a learned binary decision

### The rectified sigmoid and the annealing schedule

### Why "round to nearest" is not the argmin

## AdaQuant

## BRECQ

### Block-wise rather than layer-wise

### The Fisher-information argument for the block boundary

## QDrop

### Dropping activation quantization during reconstruction

## OBQ and the Path to GPTQ

## Cost and Quality Comparison

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 12 — QAT Foundations](./12_qat_foundations.md) — when reconstruction is not enough and you must train.
