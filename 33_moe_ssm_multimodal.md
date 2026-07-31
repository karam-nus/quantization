---
title: "Chapter 33 — MoE, SSMs & Multimodal"
---

[← Back to Table of Contents](./README.md)

# Chapter 33 — MoE, SSMs & Multimodal

> *"A mixture of experts is not one model to quantize. It is a hundred models with wildly different amounts of traffic."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part IV.

## Overview

## Mixture of Experts

### Per-expert scales

### The shared/dense expert needs higher precision

### Router logits stay in float

### Why expert weights look quantization-friendly — and where that intuition fails

## State-Space Models

### Mamba's recurrence and the selective-scan numerics

### Quamba

## Vision-Language Models

### The vision tower and the language tower have different sensitivity profiles

### Quantize them separately

## Embedding and Reranker Models

### Output-vector geometry matters more than any token-level metric

### Evaluating with retrieval metrics

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 34 — Do LLM Methods Work on CNNs?](./34_cross_pollination.md) — the chapter that closes the loop back to Part III.
