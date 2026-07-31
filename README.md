---
title: "Quantization — The Complete Guide"
permalink: /
---

# 🔢 Quantization — The Complete Guide

> **From the Affine Map to the Accelerator**: how deep networks are compressed into integers, why it works, when it breaks, and what actually runs faster. A rigorous reference covering CNNs and LLMs, calibration through kernels.

## Who This Is For

You know PyTorch and linear algebra. You have probably run `load_in_4bit=True` or exported an INT8 TFLite model and it either worked or it didn't — and you want to know **why**. This book treats quantization as what it is: one simple formula surrounded by a large, sharp-edged design space of calibration algorithms, granularities, numeric formats, graph representations, and hardware contracts. Every technique is derived, every number is cited, and every method is given with its failure modes.

The authoring standard for this book is checked in at [PROMPT.md](./PROMPT.md).

## 📋 Table of Contents

| # | Chapter | What You'll Learn |
|---|---------|-------------------|
| **Orientation** | | |
| 0 | [The Whole Field on One Page](./00_grand_overview.md) | The six independent axes of any quantization decision — what, where, when, format, algorithm, hardware |
| **Part I — Foundations** | | |
| 1 | [Hardware Compatibility & the Q/DQ Contract](./01_hardware_and_the_qdq_contract.md) | What accelerators actually execute, why Q/DQ nodes exist, when they're free and when they cost, accumulators, requantization — and whether you need quantization at all |
| 2 | [Number Formats & Bit Layouts](./02_number_formats.md) | FP32→FP4, INT8→ternary, NF4, MXFP4, NVFP4 — bit layouts, ranges, and micro-scaling |
| 3 | [The Quantization Formula, Derived](./03_the_quantization_formula.md) | Affine and symmetric maps, rounding, the clipping/rounding trade-off, SQNR, integer-only inference worked end to end |
| 4 | [Granularity — Where the Scales Live](./04_granularity.md) | Per-tensor → per-channel → per-group → **per-block** → per-token → micro-scaling, and the overhead arithmetic of each |
| 5 | [Q/DQ in the Graph](./05_graph_representation.md) | Fake vs real quant, ONNX QDQ vs QOperator, BN folding, INT32 bias, residual scale sharing |
| 6 | [Calibration — Choosing the Range](./06_calibration.md) | Min-max, percentile, histogram, **KL-divergence**, MSE, cosine, Hessian-weighted — implemented, compared, and when each fails |
| 7 | [Choosing the Targets](./07_what_to_quantize.md) | Weights vs activations vs KV-cache vs gradients — what each one actually buys you |
| **Part II — Methods & Regimes** | | |
| 8 | [Beyond PTQ vs QAT](./08_method_taxonomy.md) | The six axes people miss: data-free, reconstruction, dynamic/static, codebook, bit-search, and model-transform methods |
| 9 | [Dynamic vs Static Quantization](./09_dynamic_vs_static.md) | Per-token ranges at runtime — cheap for LLM decode, expensive for CNN latency |
| 10 | [Data-Free & Zero-Shot](./10_data_free_quantization.md) | Cross-layer equalization, bias correction, ZeroQ, SQuant, HQQ — when you have no calibration data |
| 11 | [Layer-wise Reconstruction PTQ](./11_reconstruction_ptq.md) | AdaRound, AdaQuant, BRECQ, QDrop, OBQ — learned rounding and the reconstruction objective |
| 12 | [QAT Foundations](./12_qat_foundations.md) | STE and its bias, LSQ, LSQ+, PACT, DSQ, TQT, BN handling, and recipes that converge |
| 13 | [Quantization-Aware Distillation](./13_quantization_aware_distillation.md) | Quantized student, FP teacher — where the second forward pass pays for itself |
| 14 | [Mixed Precision & Bit Allocation](./14_mixed_precision_search.md) | Sensitivity analysis, HAWQ, HAQ, Pareto frontiers, and how EXL2/GGUF do it in practice |
| 15 | [The Outlier Problem](./15_outliers.md) | Why activation outliers exist, why they're lethal, and the four families of fixes: clip, isolate, equalize, rotate |
| 16 | [Non-Uniform & Codebook Quantization](./16_nonuniform_and_codebook.md) | k-means, VQ/PQ, AQLM, QuIP# lattices, NF4, log grids — and the kernel cost they all share |
| **Part III — CNN & Vision** | | |
| 17 | [What Breaks in a ConvNet](./17_cnn_quantization_anatomy.md) | Depthwise convs, SE blocks, residual/concat requant, ReLU6 vs Swish, first/last layer policy |
| 18 | [CNN PTQ Recipes](./18_cnn_ptq_practice.md) | The canonical config, target-specific variations, and the escalation ladder when INT8 isn't enough |
| 19 | [CNN QAT Recipes](./19_cnn_qat_practice.md) | Epoch budgets, LR, observer/BN freezing, recovering MobileNetV3 |
| 20 | [Detection & Dense Prediction](./20_detection_and_dense_prediction.md) | Why detectors quantize worse than classifiers — regression heads, NMS, mAP degradation patterns |
| 21 | [Quantizing Vision Transformers](./21_vision_transformers.md) | PTQ4ViT, FQ-ViT, RepQ-ViT — post-LayerNorm and post-Softmax distributions, the bridge to LLMs |
| **Part IV — LLM Quantization** | | |
| 22 | [Why LLMs Are a Different Problem](./22_why_llms_differ.md) | Memory-bound decode, the roofline argument, and why weight-only is enough for LLMs but not CNNs |
| 23 | [Weight-Only Baselines](./23_weight_only_baselines.md) | LLM.int8(), NF4 + double quant, HQQ, Quanto, TorchAO — the zero-calibration options |
| 24 | [GPTQ](./24_gptq.md) | OBS → OBC → GPTQ derived: the Hessian, the column update, Cholesky, act-order, group size, damping |
| 25 | [AWQ](./25_awq.md) | Activation-aware salience, the diagonal transform equivalence, the scale search, GEMM vs GEMV |
| 26 | [SmoothQuant & Activation-Weight Balancing](./26_smoothquant.md) | The α migration rule derived, folding into LayerNorm, and its direct lineage from cross-layer equalization |
| 27 | [Rotation & Incoherence](./27_rotation_methods.md) | QuaRot, SpinQuant, FlatQuant, DuQuant — Hadamard transforms, computational invariance, learned rotations |
| 28 | [W4A8 & Full-Stack Schemes](./28_w4a8_and_fullstack.md) | QoQ/QServe, Atom, ZeroQuant, OmniQuant — making dequantization cheap is the real problem |
| 29 | [FP8 & FP4](./29_fp8_and_fp4.md) | E4M3/E5M2 recipes, delayed scaling, Transformer Engine, NVFP4 vs MXFP4 compared directly |
| 30 | [2-Bit and Below](./30_extreme_low_bit.md) | QuIP#, AQLM, PB-LLM, BitNet b1.58, OneBit — and where quality honestly collapses |
| 31 | [KV-Cache Quantization](./31_kv_cache_quantization.md) | KIVI, KVQuant, per-channel K / per-token V, FP8 KV, residual windows, PagedAttention interaction |
| 32 | [Fine-Tuning Quantized Models](./32_quantized_finetuning.md) | QLoRA, LoftQ, QA-LoRA, EfficientQAT, BitDelta — and the merge-time precision trap |
| 33 | [MoE, SSMs & Multimodal](./33_moe_ssm_multimodal.md) | Per-expert scales, router precision, Mamba/Quamba, vision vs language towers, embedding models |
| 34 | [Do LLM Methods Work on CNNs?](./34_cross_pollination.md) | **The lineage is CNN → LLM.** What transfers (conv-as-GEMM, Hessians, grouping), what doesn't (depthwise, spatial outliers, missing kernels), and the recipe |
| **Part V — Formats** | | |
| 35 | [GGUF I — Container & Block Quantization](./35_gguf_part1.md) | Why GGUF is **block-based, not channel-based**, and the exact bit layout + dequant formula for Q4_0…Q8_0 |
| 36 | [GGUF II — K-Quants, I-Quants & Choosing](./36_gguf_part2.md) | Superblocks and two-level scales, Q2_K…Q6_K byte by byte, IQ codebooks, ternary types, the imatrix, and the full selection table |
| 37 | [Every Other Format](./37_other_formats.md) | compressed-tensors, GPTQ/AWQ packing, EXL2/3, ONNX QDQ, TFLite, TensorRT, MLX, CoreML, QNN — with the master granularity table |
| **Part VI — Frameworks & Kernels** | | |
| 38 | [PyTorch Quantization](./38_pytorch_quantization.md) | Eager → FX → **PT2E**, observers, qconfig, custom observers, TorchAO's `quantize_` |
| 39 | [Vendor & Edge Toolchains](./39_deployment_toolchains.md) | TensorRT, ONNX Runtime, OpenVINO/NNCF, TFLite, QNN/SNPE/AIMET, Brevitas, Ethos-U, DRP-AI, Hailo, CoreML |
| 40 | [Serving Stacks](./40_llm_serving_stacks.md) | vLLM, SGLang, TensorRT-LLM, llama.cpp, ExLlamaV2, MLC, TGI — which format each can actually run |
| 41 | [How a Quantized Matmul Runs](./41_quantized_kernels.md) | dp4a and integer tensor cores, Marlin/Machete, dequant-fused GEMV, and why W4A16 stops helping at large batch |
| **Part VII — Practice** | | |
| 42 | [Hands-On: Quantizing a CNN](./42_handson_cnn_ptq.md) | Calibrators from scratch, per-channel weights, SQNR, and watching MobileNetV2 fail and recover |
| 43 | [Hands-On: Export & Deploy](./43_handson_export_and_deploy.md) | PT2E → ONNX QDQ → ONNX Runtime and TensorRT INT8, and proving the integer kernels really ran |
| 44 | [Hands-On: QAT From Scratch](./44_handson_qat.md) | An STE autograd function and an LSQ quantizer written by hand, then checked against the library |
| 45 | [Hands-On: Quantizing an LLM](./45_handson_llm.md) | GPTQ, AWQ, SmoothQuant, FP8 on one model — served on vLLM and measured |
| 46 | [Hands-On: GGUF End to End](./46_handson_gguf.md) | Convert → imatrix → quantize → perplexity + bench, then read a block back with NumPy |
| 47 | [The Debugging Playbook](./47_debugging_playbook.md) | Symptom → cause → diagnostic → fix. Per-layer SQNR bisection, sensitivity sweeps, cross-runtime mismatch hunting |
| 48 | [Evaluating Honestly](./48_evaluating_quantized_models.md) | Why KL-divergence to the FP model beats perplexity, latency/TTFT/energy, the memory formula, and how comparisons mislead |
| 49 | [The Decision Guide](./49_decision_guide.md) | Flowcharts by starting point, each terminating in a concrete recipe and a chapter link |
| **Appendices** | | |
| A | [Glossary & Symbols](./appendix_a_glossary.md) | Every term and symbol, one canonical definition each |
| B | [Math Reference](./appendix_b_math.md) | MSE-optimal scale, optimal clipping, KL objective, OBS update, STE bias, effective-bits accounting |
| C | [Method Index](./appendix_c_method_index.md) | The master table — every method × year × class × bits × granularity × hardware × tooling |
| D | [Hardware Capability Matrix](./appendix_d_hardware_matrix.md) | Accelerator × precisions × granularity limits × accumulator × toolchain |
| E | [Bibliography](./appendix_e_bibliography.md) | The full reading list by theme, with a suggested order |

## 🗺️ Learning Path

<div class="diagram">
<div class="diagram-title">Recommended Reading Order</div>
<div class="flow">
  <div class="flow-node accent wide">🧭 Ch 0–7: Hardware Contract & Foundations <small>the formula, granularity, calibration</small></div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node purple wide">🧩 Ch 8–16: Methods & Regimes <small>PTQ, QAT, reconstruction, outliers</small></div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node green wide">🖼️ Ch 17–21: CNNs & Vision Transformers</div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node orange wide">💬 Ch 22–34: LLM Quantization <small>GPTQ, AWQ, SmoothQuant, rotations, FP4, KV</small></div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node cyan wide">📦 Ch 35–37: GGUF & Serialization Formats</div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node teal wide">🛠️ Ch 38–41: Frameworks, Toolchains & Kernels</div>
  <div class="flow-arrow accent"></div>
  <div class="flow-node pink wide">🔬 Ch 42–49: Hands-On, Debugging & Decisions</div>
</div>
</div>

## ⚡ Quick Start Paths

### Path A: "I deploy CNNs on an NPU or embedded target"

1. [01 — Hardware & the Q/DQ Contract](./01_hardware_and_the_qdq_contract.md) — what your target can actually execute
2. [03 — The Quantization Formula](./03_the_quantization_formula.md) — integer-only inference and requantization
3. [06 — Calibration](./06_calibration.md) — pick the right calibrator for your distributions
4. [17 — What Breaks in a ConvNet](./17_cnn_quantization_anatomy.md) — depthwise convs and friends
5. [18 — CNN PTQ Recipes](./18_cnn_ptq_practice.md) → [19 — CNN QAT](./19_cnn_qat_practice.md)
6. [39 — Vendor & Edge Toolchains](./39_deployment_toolchains.md) — your specific SDK
7. [47 — The Debugging Playbook](./47_debugging_playbook.md) — when the accuracy drops

### Path B: "I serve LLMs on GPUs"

1. [22 — Why LLMs Are Different](./22_why_llms_differ.md) — the roofline argument
2. [15 — The Outlier Problem](./15_outliers.md) — the constraint everything else works around
3. [24 — GPTQ](./24_gptq.md) → [25 — AWQ](./25_awq.md) → [26 — SmoothQuant](./26_smoothquant.md)
4. [29 — FP8 & FP4](./29_fp8_and_fp4.md) — the native-hardware path
5. [31 — KV-Cache Quantization](./31_kv_cache_quantization.md) — where the memory actually goes
6. [41 — How a Quantized Matmul Runs](./41_quantized_kernels.md) — why your speedup is smaller than expected
7. [45 — Hands-On: Quantizing an LLM](./45_handson_llm.md)

### Path C: "I run models locally with llama.cpp / Ollama"

1. [04 — Granularity](./04_granularity.md) — block-wise vs channel-wise, the key idea
2. [35 — GGUF I](./35_gguf_part1.md) — the container and the legacy block types
3. [36 — GGUF II](./36_gguf_part2.md) — K-quants, I-quants, imatrix, and the selection table
4. [46 — Hands-On: GGUF End to End](./46_handson_gguf.md)
5. [48 — Evaluating Honestly](./48_evaluating_quantized_models.md) — is Q4_K_M actually good enough for you?

### Path D: "Something is broken and I need to fix it"

1. [47 — The Debugging Playbook](./47_debugging_playbook.md) — start here, it's symptom-indexed
2. [05 — Q/DQ in the Graph](./05_graph_representation.md) — export and semantics mismatches
3. [06 — Calibration](./06_calibration.md) — bad calibration is the most common root cause
4. [15 — The Outlier Problem](./15_outliers.md) — the second most common
5. [48 — Evaluating Honestly](./48_evaluating_quantized_models.md) — make sure you're measuring the right thing

### Path E: "I want the theory, properly"

1. [03 — The Quantization Formula](./03_the_quantization_formula.md)
2. [11 — Layer-wise Reconstruction PTQ](./11_reconstruction_ptq.md)
3. [12 — QAT Foundations](./12_qat_foundations.md)
4. [24 — GPTQ](./24_gptq.md) — the cleanest derivation in the book
5. [27 — Rotation & Incoherence](./27_rotation_methods.md)
6. [Appendix B — Math Reference](./appendix_b_math.md)

---

*Part of a knowledge base published at [karam-nus.github.io](https://karam-nus.github.io). Companion repos: [language-modelling](https://karam-nus.github.io/language-modelling), [inference](https://karam-nus.github.io/inference), [model-optimization](https://karam-nus.github.io/model-optimization), [vision](https://karam-nus.github.io/vision).*
