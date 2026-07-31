---
title: "Chapter 34 — Do LLM Methods Work on CNNs?"
---

[← Back to Table of Contents](./README.md)

# Chapter 34 — Do LLM Methods Work on CNNs?

> *"They are not LLM techniques. They are linear-layer techniques that became famous on LLMs — and most of them were invented on ConvNets."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV. Structure this as an argument, not a list. Requires a dedicated research pass; be explicit where published evidence is thin rather than inventing results.

## Overview

## Part 1 — The Lineage Runs CNN → LLM

### GPTQ is OBQ is Optimal Brain Surgeon, developed and evaluated on ConvNets

### SmoothQuant's migration is cross-layer equalization, published on MobileNet in 2019

### AWQ's salience weighting is sensitivity-weighted PTQ

### AdaRound and BRECQ were built for CNNs and later reused wholesale for LLMs

### Hadamard and incoherence processing are classical decorrelation

### Timeline

## Part 2 — What Transfers, Mechanically

### Conv as GEMM: `im2col`/unfold and the input dimension $C_{in}k_hk_w$

### Hessian estimation accumulating over spatial positions as well as batch

### Per-group scaling along the input dimension: when the group is large enough

### Equalization needs a scale-equivariant path to fold into

### Rotations need a residual stream with a consistent channel basis

## Part 3 — What Does Not Transfer

### Depthwise convolutions have too few weights per output channel

### CNN activation outliers are spatial and input-dependent, not channel-consistent

### No CNN runtime executes a group-quantized convolution

### The win is compression, not latency

### BatchNorm statistics interacting with modified weights

## Part 4 — The Recipe

### GPTQ-style reconstruction applied to a ViT

### The same applied to the 1×1 convolutions of a ResNet

### The unfold, the Hessian accumulation, the group assignment

### What to expect, and what the published work reports

## Part 5 — The Decision Table

### Method × (CNN / depthwise CNN / ViT / LLM) → transfers cleanly / with modification / not at all

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 35 — GGUF I](./35_gguf_part1.md) — Part V turns from algorithms to the bytes on disk.
