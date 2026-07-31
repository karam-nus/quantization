---
title: "Chapter 8 — Beyond PTQ vs QAT"
---

[← Back to Table of Contents](./README.md)

# Chapter 8 — Beyond PTQ vs QAT

> *"PTQ versus QAT is one axis of six. Most confusion in this field comes from collapsing the other five."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part II.

## Overview

## Axis 1 — Data Requirement

### Data-free → calibration-only → reconstruction (unlabeled) → full retraining

## Axis 2 — Timing

### Post-training → quantization-aware fine-tuning → QAT → native low-bit pre-training

## Axis 3 — Static vs Dynamic Activation Ranges

## Axis 4 — Uniform vs Non-Uniform / Codebook

## Axis 5 — Fixed vs Searched Bit Allocation

## Axis 6 — Model Transformation Before Quantization

### Equalization, rotation, reparameterization — neither PTQ nor QAT

## The Orthogonal Question: Who Applies It

### Framework at load time, offline pipeline, or compiler

## Master Decision Tree

## Every Method in This Book, Placed on These Axes

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 9 — Dynamic vs Static Quantization](./09_dynamic_vs_static.md) — the axis with the largest practical consequences.
