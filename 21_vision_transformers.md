---
title: "Chapter 21 — Quantizing Vision Transformers"
---

[← Back to Table of Contents](./README.md)

# Chapter 21 — Quantizing Vision Transformers

> *"ViTs sit between CNNs and LLMs on every axis — which is exactly why techniques cross between them."*

**Status: stub.** Written per [PROMPT.md](./PROMPT.md) §6, Part III.

## Overview

## The Distributions That Cause Trouble

### Post-LayerNorm activations

### Post-Softmax long tails

### GELU's asymmetry

## PTQ4ViT

### Twin uniform quantization

### Hessian-guided scale search

## FQ-ViT

### Log2 softmax quantization

### Power-of-two-factor LayerNorm

## RepQ-ViT

### Scale reparameterization from per-channel to per-tensor

## Where ViTs Sit Between CNNs and LLMs

### The setup for Chapter 34

## Key Takeaways

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter 22 — Why LLMs Are a Different Problem](./22_why_llms_differ.md) — Part IV begins.
