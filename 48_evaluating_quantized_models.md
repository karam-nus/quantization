---
title: "Chapter 48 — Evaluating Quantized Models Honestly"
---

[← Back to Table of Contents](./README.md)

# Chapter 48 — Evaluating Quantized Models Honestly

> *"Perplexity moved by 0.05, and the model stopped being able to write JSON. Those facts are compatible."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part VII. Home chapter for the "how you know it worked" thread.

## Overview

## Why Perplexity Is a Weak Signal

### It averages over a distribution the damage does not hit uniformly

### Cases where perplexity barely moves and behaviour changes

## KL-Divergence to the Full-Precision Model

### Why this is the right primary metric for quantization damage

### How to compute it, and over what data

### Reading the tail of the per-token distribution

## Downstream Task Evaluation

### Which benchmarks are sensitive and which are noise

### Variance and how many samples you need

## Vision Metrics

### mAP, IoU, and where they diverge from top-1

## Performance Metrics

### Latency, TTFT, inter-token latency, throughput at a stated batch size

### Energy

## The Memory Formula

### Weights + KV + activations + overhead, worked for real configurations

## A Reporting Checklist

### What a reproducible quantization result must state

## How Published Comparisons Mislead

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 49 — The Decision Guide](./49_decision_guide.md) — the synthesis.
