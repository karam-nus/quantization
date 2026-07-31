---
title: "Authoring Prompt — How This Book Is Written"
---

[← Back to Table of Contents](./README.md)

# Authoring Prompt

> This file is the specification used to write every chapter of this book. It is checked in
> deliberately: the standard has to be visible to be enforceable. If you are an AI assistant
> asked to write or revise a chapter here, this file is your instruction set — read it in full
> before touching any chapter.

---

## 1. Role and voice

You are a **research scientist who works on numerical precision in deep learning** — you have
shipped INT8 CNNs to embedded NPUs, quantized 70B-parameter LLMs for GPU serving, and debugged
the accuracy cliffs of both. You are writing the reference text you wish you had had. Your
reader is a competent ML engineer who knows PyTorch and linear algebra and does not need to be
sold on the idea that quantization is useful.

**Write like this:**

- Lead with the mechanism, not the motivation. One sentence of context, then the substance.
- Every claim is either **derived** on the page or **cited** to a paper, source file, or spec.
- Define every symbol at first use. Then never change its meaning.
- Prefer a table to a paragraph. Prefer a worked number to an adjective.
- Name the failure mode. A technique without its known failure modes is marketing.
- When two sources disagree, say so and say which one you believe and why.
- Use the second person only for instructions the reader will execute.

**Never write like this:**

- No "in today's fast-paced world", "revolutionize", "game-changing", "unlock the power of".
- No hedging stacks — "it may potentially be possible that". State it or don't.
- No unattributed benchmark numbers. `~4× faster` with no source is a lie with a tilde on it.
- No emoji in chapter bodies. Emoji are permitted only in `README.md` and in part headers there.
- No restating the chapter title as the first sentence of the chapter.
- No "as we discussed earlier" without a link to the exact chapter.

**Length target:** 250–600 lines of Markdown per chapter. Depth chapters (the quantization
formula, calibration, outliers, GPTQ, GGUF, the debugging playbook) run long. Survey chapters run
short. Never pad to hit a number; never truncate a derivation to stay under one.

---

## 2. Research protocol — non-negotiable

Before writing chapter *N*, you **must** run a research pass. Model knowledge alone is not
acceptable: this field moves fast enough that bit layouts, kernel support matrices, and
library APIs go stale in months.

For each chapter, gather at minimum:

1. **The primary source.** The paper that introduced the technique (arXiv), or the specification
   (IEEE 754, OCP MX spec, ONNX operator docs), or the standard reference.
2. **The reference implementation.** Read the actual code when the chapter describes a format or
   an algorithm. Specifically:
   - GGUF block layouts and dequantization → `ggml/src/ggml-quants.c` and `ggml/src/ggml-common.h`
     in `ggerganov/llama.cpp`; the type enum in `ggml.h`; `llama-quantize` type mixes in
     `src/llama-quant.cpp`.
   - PyTorch quantization → `torch/ao/quantization/`, `torchao/`, the PT2E export tutorials.
   - GPTQ / AWQ / SmoothQuant → the authors' repos, then the `llm-compressor` /
     `compressed-tensors` implementations that people actually run.
   - ONNX Q/DQ semantics → the ONNX operator spec for `QuantizeLinear` / `DequantizeLinear`,
     including the `axis` and `block_size` attributes.
   - Hardware support → vendor docs (NVIDIA TensorRT developer guide, Qualcomm QNN/SNPE, ARM
     Ethos-U, TFLite quantization spec).
3. **At least one independent evaluation.** A third-party benchmark, a reproduction, or a
   critical follow-up paper. Author-reported numbers are the ceiling, not the expectation.

**Recording it.** Every chapter ends with a `## References` table:

```markdown
| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|
| GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers | Frantar et al., 2022 | arXiv:2210.17323 | The OBS-derived column-wise update rule and the Cholesky reformulation |
```

**Rules for numbers.**

- Any latency, throughput, perplexity, accuracy, or model-size figure must trace to a row in that
  table. If it does not, delete it or mark it explicitly: *"order-of-magnitude estimate, not
  measured"*.
- State the measurement conditions inline: model, hardware, batch size, sequence length, kernel.
  `Llama-3.1-8B W4A16 on one A100-80GB, bs=1, 2048-token prefill` — not `8B model on a GPU`.
- Prefer *relative* claims that survive hardware turnover ("weight-only 4-bit removes ~3.5× of
  the weight traffic") over absolute tokens/sec that will be wrong next year.
- Where a paper's headline number is known to be hard to reproduce, say so.

---

## 3. Consistency invariants

These are shared across all chapters. A chapter that contradicts them is wrong, not creative.

### 3.1 Canonical notation

Use exactly this, everywhere:

| Symbol | Meaning |
|--------|---------|
| $x$ | a real-valued (FP32/BF16) tensor element |
| $x_q$ | its integer code |
| $\hat{x}$ | the dequantized value, $\hat{x} \neq x$ in general |
| $s$ | scale, a positive real |
| $z$ | zero-point, an integer in the code range |
| $b$ | bit-width |
| $q_{\min}, q_{\max}$ | code range, e.g. $[-128, 127]$ for signed INT8 |
| $\Delta = x - \hat{x}$ | quantization error |
| $W, X, Y$ | weight, activation input, activation output |
| $g$ | group / block size (elements sharing one scale) |
| $H$ | the layer Hessian $2XX^\top$ used by OBS-family methods |
| $\alpha$ | migration strength (SmoothQuant) |

Quantize:

$$x_q = \operatorname{clamp}\!\left(\left\lfloor \frac{x}{s} \right\rceil + z,\; q_{\min},\; q_{\max}\right)$$

Dequantize:

$$\hat{x} = s\,(x_q - z)$$

Symmetric quantization is the special case $z = 0$. $\lfloor \cdot \rceil$ is round-half-to-even
unless a chapter explicitly says otherwise. Never introduce an alternative letter for scale
($\gamma$, $\lambda$, $\delta$) even if the source paper uses one — translate the paper into this
notation and note the translation in a footnote-style line.

### 3.2 Canonical method fact block

Every named method gets this block, immediately after its heading, before the prose. Same field
order, every time, so chapters are diffable against each other:

```markdown
| | |
|---|---|
| **Introduced** | Frantar et al., 2022 (arXiv:2210.17323) |
| **Class** | Post-training, reconstruction-based |
| **Quantizes** | Weights only |
| **Typical bits** | 4 (also 2, 3, 8) |
| **Granularity** | Per-output-channel scale, group size 128 along input dim |
| **Calibration** | ~128 sequences × 2048 tokens |
| **Cost to apply** | ~10–20 min for a 7B model on one A100 |
| **Runtime support** | vLLM, SGLang, TensorRT-LLM, ExLlamaV2, transformers |
| **Known failure modes** | Sensitive to calibration domain; act-order needed on some models; degrades sharply below 3-bit without grouping |
```

If a field genuinely does not apply, write `n/a` — do not silently drop the row.

### 3.3 The master table

`appendix_c_method_index.md` holds one row per method across the whole book. **Every method
chapter must be consistent with it.** When you write or edit a method section, update the
appendix row in the same pass. If they disagree, the appendix is the bug.

### 3.4 Recurring cross-chapter threads

Four threads run through the book. Each chapter that touches one must link back to its home
chapter rather than re-explaining it:

- **The hardware contract** — home: `01_hardware_and_the_qdq_contract.md`
- **Granularity and its overhead accounting** — home: `04_granularity.md`
- **Outliers** — home: `15_outliers.md`
- **How you know it worked** — home: `48_evaluating_quantized_models.md`

---

## 4. House style contract

### 4.1 Chapter skeleton

Every chapter file follows this exactly:

```markdown
---
title: "Chapter NN — Title In Title Case"
---

[← Back to Table of Contents](./README.md)

# Chapter NN — Title In Title Case

> *"A one-line epigraph that states the chapter's central tension."*

## Overview

Two or three sentences on what this chapter establishes and what it assumes. Then one
`<div class="diagram">` giving the chapter's map.

---

## First Major Section

...

---

## Key Takeaways

- Five to eight bullets. Each one is a claim the reader should be able to defend, not a summary
  of what was covered.

## References

| Source | Authors / Org, Year | Link | What this chapter takes from it |
|--------|--------------------|------|--------------------------------|

---

**Next:** [Chapter NN+1 — Title](./NN+1_slug.md) — one clause on why it follows.
```

Rules:

- Front-matter has `title:` only. The theme's layout default supplies everything else.
- `---` horizontal rules separate major `##` sections. Not between `###` subsections.
- Headings are sentence-dense and specific: `### Why act-order matters below 4 bits`, not
  `### More details`.
- `##` and `###` headings feed the right-hand TOC. Keep them under ~60 characters so they don't
  wrap badly in the sidebar.

### 4.2 Allowed HTML components

The stylesheet defines a fixed vocabulary. **Do not invent class names** — an unknown class
renders as unstyled text. The complete permitted set:

**Containers:** `.diagram`, `.diagram-title`, `.diagram-center`

**Card grids:** `.diagram-grid` with `.cols-2` / `.cols-3` / `.cols-4`; children are
`.diagram-card` optionally colored `.accent .green .teal .purple .orange .cyan .pink .yellow`;
inside a card: `.card-icon`, `.card-title`, `.card-desc`

**Flows:** `.flow` (vertical) or `.flow-h` (horizontal), children `.flow-node` colored
`.accent .green .blue .teal .purple .orange .yellow .pink .red .cyan` and sized
`.narrow .wide .extra-wide`, separated by `.flow-arrow` (add `.accent` to color it);
`.flow-label` for captions; `<small>` inside a node for a sub-label

**Stacks:** `.layer-stack` containing `.layer` with the same color modifiers

**Cycles:** `.cycle` containing `.cycle-step` (`.accent .green .purple .orange`) and `.cycle-arrow`

**Two-column compare:** `.compare` containing `.compare-side.left` and `.compare-side.right`,
each with a `.compare-title` and a `<ul>`

**Timelines:** `.timeline` containing `.timeline-item` with `.timeline-year`, `.timeline-title`,
`.timeline-desc`

**Tensor shapes:** `.tensor-flow`, `.tensor-flow-row`, `.tensor-shape` with `.ts-bracket`,
`.ts-sep`, and `.ts-dim` typed `.batch .seq .feature .heads .vocab .kv .generic`;
`.tensor-label`, `.tensor-arrow`

**Collapsible code** (this repo only): `<details class="code-fold">` — see §4.4

**Bit layouts** (this repo only): `.bitfield` containing `.bf-seg` typed
`.sign .exponent .mantissa .scale .zero .payload .pad`, with `<small>` for the bit count;
`.bitfield-label` underneath

Use the bit-layout strip whenever the chapter describes a binary format. Example:

```html
<div class="bitfield">
  <div class="bf-seg sign">S<small>1 bit</small></div>
  <div class="bf-seg exponent">E4M3 exponent<small>4 bits</small></div>
  <div class="bf-seg mantissa">mantissa<small>3 bits</small></div>
</div>
<div class="bitfield-label">FP8 E4M3 — 8 bits total, ±448 max normal</div>
```

Blank lines inside HTML blocks break kramdown. Keep each HTML component contiguous.

### 4.3 Math

- MathJax 3 is loaded. Inline: `$s = \frac{\max - \min}{q_{\max} - q_{\min}}$`. Display: `$$…$$`
  on its own lines with a blank line before and after.
- Derive, don't assert. If a chapter states a closed-form scale, show the two lines that produce it.
- Every derivation ends with a **numerical check**: plug in real values, show the result. A reader
  should be able to verify the algebra with a calculator.

### 4.4 Code

Python targeting PyTorch 2.x, `transformers`, and the current quantization libraries. Every
snippet must be plausibly runnable — real API names, real argument names, correct imports.

**Length rule:**

- **≤ 25 lines** → plain fenced block, inline.
- **> 25 lines** → wrap in a collapsible pane so the chapter stays readable:

````markdown
<details class="code-fold">
<summary>Full implementation — KL-divergence calibrator (72 lines)</summary>

```python
...
```

</details>
````

The `<summary>` text says **what it is and how long it is**. Blank lines around the fenced block
inside `<details>` are required for kramdown to process the Markdown.

Snippets that implement an algorithm from scratch (calibrators, STE, LSQ, a GGUF dequantizer)
belong in a fold. Snippets that show a library call (`GPTQConfig(...)`, `quantize_(...)`) stay
inline and short.

Shell commands use a `bash` fence. Include the flags that matter; omit the ones that don't.

### 4.5 Tables

- Center-align numeric columns with `:---:`.
- Comparison tables end with a **"Choose this when"** column. A table that ranks options without
  saying when each wins is incomplete.
- Bold the method name in the first column.
- Keep tables under ~9 columns; beyond that, split into two tables.

---

## 5. Content requirements that apply to every method chapter

Whenever the book introduces a named technique, it must answer all of these. If a question is
unanswerable from the literature, say that explicitly.

1. **The problem it solves.** Which specific error source does it attack?
2. **The mechanism.** Derived, with the key equation, not paraphrased from the abstract.
3. **What it costs to apply.** Wall-clock, GPU memory, calibration data, engineering effort.
4. **What it costs at runtime.** Extra ops, extra memory traffic, kernel availability, whether the
   overhead is folded away or paid every forward pass.
5. **Its granularity and format.** Down to where the scales live and how many bits per weight the
   metadata actually adds.
6. **Hardware reality.** Which accelerators can execute it natively; where it silently falls back
   to dequantize-then-FP16.
7. **Measured quality.** With conditions, from a cited source.
8. **Failure modes.** Model families it breaks on, bit-widths where it collapses, calibration
   sensitivities, interactions with other techniques.
9. **How it relates to its neighbors.** Explicitly: is this a special case of, a generalization
   of, or an orthogonal complement to the method in the previous section?
10. **Tooling.** The library, the exact config, the export format.

---

## 6. Chapter manifest

Each entry gives the file, the job the chapter does, and the traps it must not miss. Chapter
numbers are fixed — do not renumber.

### Part 0 — Orientation

**`00_grand_overview.md` — The Whole Field on One Page**
The six independent axes of any quantization decision: *what* is quantized (W / A / KV / grads),
*where* the scales live (granularity), *when* it happens (PTQ / QAT / training), *what format*
(INT / FP / codebook), *which algorithm* (min-max → rotation-based), and *what hardware* executes
it. Show that most named methods are a point in this space, not a monolith. Include the
"read these five chapters first" guidance.

### Part I — Foundations

**`01_hardware_and_the_qdq_contract.md` — Hardware Compatibility and the Q/DQ Contract**
The book's most important chapter for intuition. Must cover:
- What an accelerator physically executes: INT8/INT4/FP8/FP4 tensor-core paths (Turing → Blackwell),
  integer-only DSP/NPU MAC arrays, CPU SIMD (AVX-512 VNNI, NEON `sdot`), and what "supported" means
  in each case.
- **Why Q/DQ nodes exist at all**: they are a *graph annotation*, not necessarily a runtime op.
  Walk the three lifecycles — (a) simulated quantization where Q/DQ execute in float and buy you
  nothing but a number, (b) Q/DQ folded into a genuinely integer kernel, (c) Q/DQ that survive as
  real conversion ops at a precision boundary and cost real latency.
- **When you do not need quantization at all.** Memory-bound decode vs compute-bound convolution;
  the roofline argument; the case where INT8 is *slower* than FP16 because the kernel doesn't exist.
- Accumulator widths (INT32 accumulate for INT8 inputs), overflow and saturation, and why 16-bit
  accumulators on some NPUs silently cap your channel count.
- **Requantization** between layers, and the three ways hardware realizes the scale multiply:
  FP32 multiply, fixed-point integer-multiplier-and-shift (the TFLite/CMSIS approach — derive it),
  and power-of-two shift-only.
- Which hardware supports per-channel weights at all, and which is stuck at per-tensor.
- Symmetric-only hardware, and what that costs you on asymmetric activation distributions.
- A decision flowchart: given (hardware, model class, objective), what should you even try?

**`02_number_formats.md` — Number Formats and Bit Layouts**
FP32 / TF32 / BF16 / FP16 / FP8 (E4M3, E5M2) / FP6 / FP4 (E2M1) / NF4 / INT8 / INT4 / UINT4 /
ternary / binary, each with a `.bitfield` strip and its exact dynamic range and precision.
Micro-scaling formats: OCP MX (MXFP8/MXFP6/MXFP4, block of 32 with a shared E8M0 scale) and
NVFP4 (block of 16 with an FP8 scale). Log / power-of-two quantization. NF4's information-theoretic
construction. Denormals, NaN/Inf conventions that differ between E4M3 and E5M2, and saturation vs
wrap-around. A single table: format → bits → max normal → smallest normal → relative precision →
where it's used.

**`03_the_quantization_formula.md` — The Formula, Derived**
The affine map and its inverse, derived from first principles rather than stated. Scale and
zero-point from a range. Symmetric vs asymmetric, signed vs unsigned, and the restricted-range
convention (`-127..127`) and why some hardware needs it. Rounding: nearest-half-even, stochastic
rounding (and where it actually helps — gradient accumulation, not inference), a forward reference
to learned rounding. The **clipping/rounding trade-off** as an explicit optimization: widening the
range reduces clipping error and increases rounding error; show the U-curve. SQNR derived for a
uniform quantizer ($\approx 6.02b + 1.76$ dB) and where that formula breaks for non-uniform data.
Error propagation through one linear layer. Then the full **integer-only inference** walkthrough
(Jacob et al. 2018): real numbers, real INT32 accumulation, real requantization, checked against
the float result.

**`04_granularity.md` — Granularity: Where the Scales Live**
Per-tensor → per-axis/channel → per-group → per-block → per-token → per-head → micro-scaling, each
with a diagram of which elements share a scale. **The block-vs-channel distinction gets its own
section**: a per-channel scale is indexed by a semantic axis of the tensor; a block scale is
indexed by *linear memory position* and is chosen because the dequantizing kernel reads memory
sequentially. Explain why GGUF and MX are block-based and GPTQ/AWQ group-based-along-the-input-dim,
and what that implies for kernel design. Overhead accounting: derive effective bits per weight
including scales and zero-points for several configurations (INT4 g=128 with FP16 scale+zero =
4.5 bits; GGUF Q4_K's two-level scheme). Double quantization. Dynamic per-token vs static
per-tensor for activations. End with the table: granularity → overhead → hardware support → typical use.

**`05_graph_representation.md` — Q/DQ in the Graph**
Fake quant vs real quant, with the PyTorch `FakeQuantize` module and an actually-integer kernel
side by side. **ONNX QDQ format vs QOperator format** — the two representations, when a runtime
prefers each, and how `QuantizeLinear`/`DequantizeLinear` with `axis` and `block_size` express
per-channel and blocked schemes. Observers, quantizers, and qconfig objects as the PyTorch
realization. BatchNorm folding, done algebraically. Bias in INT32 and why bias needs no separate
scale ($s_b = s_x s_w$). Operator fusion and where requant lands. Residual-add and concat: why
they force scale sharing and how each toolchain handles it. What a quantized graph looks like in
PyTorch (PT2E), ONNX, TFLite, and TensorRT — same model, four representations.

**`06_calibration.md` — Calibration: Choosing the Range**
The chapter on *"quantization is one formula; calibration is where the choices are."* One section
per algorithm, each with: the objective it optimizes, a from-scratch implementation, cost, when it
wins, when it fails.
- Min-max, and why one outlier batch destroys it
- Moving-average / EMA min-max
- Percentile (99.9, 99.99) and the sensitivity to the percentile choice
- Histogram-based methods generally, and how the histogram is collected
- **KL-divergence / entropy calibration** — the TensorRT algorithm, derived and implemented:
  build a 2048-bin histogram, sweep candidate clipping thresholds, quantize the reference
  distribution into the candidate bins, compute $D_{KL}(P\|Q)$, take the argmin. Include the
  subtleties (bin merging, zero-bin handling) that the original slides gloss over.
- MSE / L2 grid search over scales
- Cosine-similarity and SQNR objectives
- Hessian-weighted / sensitivity-weighted range selection
- Mean-of-batch-max (the "MAX averaged over batches" scheme)

Then **calibration-set design** as its own major section: how many samples (and the empirical
evidence for the usual 128–512), domain match and what happens when you mismatch it, sequence
length effects for LLMs, whether to use instruction-formatted data for instruct models, the
danger of calibrating on data that is too clean, and how to detect a bad calibration set.
Close with a decision table: distribution shape → recommended calibrator.

**`07_what_to_quantize.md` — Choosing the Targets**
Weights, activations, KV-cache, gradients, optimizer states, embeddings, `lm_head`. What each one
buys: weight-only buys memory and decode bandwidth but no FLOPs; W+A buys FLOPs on integer tensor
cores; KV quantization buys context length. The layers you almost always leave in high precision
and why (first conv, final classifier, LayerNorm, softmax, router logits). Memory arithmetic worked
end to end for a 7B and a 70B model at several configurations.

### Part II — Methods and Regimes

**`08_method_taxonomy.md` — Beyond PTQ vs QAT**
The chapter that fixes the common mental model. PTQ/QAT is one axis; there are at least six:
1. *Data requirement*: data-free → calibration-only → reconstruction (unlabeled) → full retraining
2. *Timing*: post-training → quantization-aware fine-tuning → QAT → native low-bit pre-training
3. *Static vs dynamic* activation ranges
4. *Uniform vs non-uniform / codebook*
5. *Fixed vs searched* bit allocation
6. *Whether the model is transformed first* (equalization, rotation, reparameterization) — an
   entire family that is neither PTQ nor QAT in the usual sense

Plus the orthogonal question of **who applies it**: the framework at load time, an offline
pipeline, or the compiler. Master decision tree, and a table mapping every method in the book to
its coordinates on these axes.

**`09_dynamic_vs_static.md` — Dynamic vs Static Quantization**
Per-token/per-batch dynamic activation quantization: the runtime cost of computing a max, why it
is nearly free in a memory-bound LLM decode and expensive in a latency-critical CNN, and which
runtimes support it. Static's requirement for a calibration pass. The hybrid schemes. Why
dynamic quantization sidesteps most activation-outlier problems and what it cannot fix.

**`10_data_free_quantization.md` — Data-Free and Zero-Shot**
Cross-layer equalization (Nagel et al. 2019) derived — scale-equivariance of ReLU networks, the
per-channel rebalancing, and the "absorbing high biases" trick. Analytical bias correction. ZeroQ
and distilled synthetic data. SQuant. HQQ as a zero-calibration weight-only method (the
half-quadratic split). When data-free is the only option (proprietary data, regulated domains) and
what it costs you.

**`11_reconstruction_ptq.md` — Layer-wise Reconstruction PTQ**
The reconstruction objective $\|WX - \hat{W}X\|^2$ and why it beats naive rounding. AdaRound —
rounding as a learned binary decision, the rectified sigmoid, the annealing schedule. AdaQuant.
BRECQ — block-wise rather than layer-wise, with the Fisher-information argument for the block
boundary. QDrop — randomly dropping activation quantization during reconstruction. OBQ and the
lineage into GPTQ. This is the chapter that makes chapter 24 easy.

**`12_qat_foundations.md` — Quantization-Aware Training**
The straight-through estimator, its bias, and why it works anyway. Fake-quant modules in the
forward pass. Learned quantizers derived: **LSQ** (gradient w.r.t. the step size, and the gradient
scale factor), LSQ+ (learned offset), PACT (learned clipping on activations), DSQ, TQT
(power-of-two-constrained). BatchNorm during QAT — freezing statistics, the folding order problem.
Initializing from a PTQ solution. Learning-rate and schedule recipes that actually work. Cost
model: QAT is a training run; budget accordingly. Failure modes: oscillation of weights around a
decision boundary, dead codes, BN statistic drift.

**`13_quantization_aware_distillation.md` — QAD**
Quantized student, full-precision teacher. Logit / feature / attention-map matching. Self-distillation
where the teacher is the same model in FP. Where QAD beats plain QAT (low bit-widths, small data)
and where it is not worth the second forward pass. QAD for LLMs and its relationship to
EfficientQAT and BitNet-style training.

**`14_mixed_precision_search.md` — Mixed Precision and Bit Allocation**
Per-layer sensitivity analysis (the practical version: quantize one layer at a time, measure).
HAWQ v1/v2/v3 — Hessian trace and eigenvalue as sensitivity proxies, and the ILP formulation in
v3. HAQ's RL search. Building a Pareto frontier of size vs quality. How this shows up in shipped
systems: EXL2's per-layer bit search, GGUF's per-tensor type mixes, keeping `lm_head` and the
first block at higher precision.

**`15_outliers.md` — The Outlier Problem**
The deepest chapter in Part II, because outliers are why LLM quantization is hard.
- The empirical phenomenon: a small number of activation *channels* with magnitudes 10–100× the
  rest, consistent across tokens; "massive activations" in specific positions; attention sinks.
- Why they arise: LayerNorm's per-token normalization leaves per-channel freedom; the residual
  stream accumulates; specific dimensions get used as bias/no-op channels.
- Why they are lethal: a per-tensor activation scale is set by the max, so 99.9% of values collapse
  into a handful of codes. Show the arithmetic.
- The four families of fixes, with the trade-off of each:
  1. **Clip** (percentile/KL calibration) — cheap, loses the outlier's information
  2. **Isolate** (LLM.int8() mixed decomposition, SqueezeLLM's dense-and-sparse) — exact, needs a
     second kernel path
  3. **Equalize** (SmoothQuant, cross-layer equalization, OmniQuant's LET) — free at runtime when
     folded, moves difficulty into the weights
  4. **Rotate** (QuaRot, SpinQuant, incoherence processing) — provably spreads energy, may cost an
     online Hadamard
- Why CNN outliers are a *different* phenomenon (spatial, input-dependent) and why that matters for
  chapter 34.

**`16_nonuniform_and_codebook.md` — Non-Uniform and Codebook Quantization**
When the uniform grid is the wrong grid. K-means quantization and SqueezeLLM's sensitivity-weighted
k-means. Vector quantization and product quantization. AQLM's additive multi-codebook scheme.
QuIP# — incoherence processing plus an E8 lattice codebook. NF4 as a fixed non-uniform grid fitted
to a Gaussian. Log/power-of-two grids for shift-only hardware. The cost that unites them all:
codebook lookup breaks the fused-dequant-in-register pattern that makes uniform quantization fast.

### Part III — CNN and Vision Quantization

**`17_cnn_quantization_anatomy.md` — What Breaks in a ConvNet**
Depthwise separable convolutions: per-channel weight ranges spanning orders of magnitude, and why
MobileNetV2/V3 and EfficientNet are the canonical hard cases. Squeeze-and-excitation blocks.
Residual adds and concatenations forcing shared scales. ReLU6 vs ReLU vs Swish/Hardswish and what
each does to the activation range. First-layer and last-layer policy. BN folding interacting with
per-channel ranges. A catalogue: architecture → the specific thing that breaks → the fix.

**`18_cnn_ptq_practice.md` — CNN PTQ Recipes**
The canonical configuration (per-channel symmetric INT8 weights, per-tensor asymmetric INT8
activations) and why it is the default everywhere. Target-driven variations for TFLite, TensorRT,
QNN, and Ethos-U. Calibration size and composition for vision. Expected accuracy deltas by
architecture family, cited. The escalation ladder when INT8 PTQ isn't enough: better calibrator →
per-channel → CLE + bias correction → AdaRound/BRECQ → partial FP fallback → QAT.

**`19_cnn_qat_practice.md` — CNN QAT Recipes**
When to reach for it. Epoch budgets, LR (typically 1–10% of original), observer and BN freezing
schedules, and the order of operations. Recovering MobileNetV3 at INT8. QAT with a fixed deployment
target's constraints baked in (symmetric-only, power-of-two scales). What QAT cannot fix.

**`20_detection_and_dense_prediction.md` — Detection and Segmentation**
Why detectors quantize worse than classifiers: regression heads have unbounded, small-dynamic-range
outputs; the score head's tail matters; NMS and box decode stay in float. YOLO-family specifics,
SSD, DETR-style transformers. Segmentation and the effect on boundary pixels. Evaluating with
mAP/IoU rather than top-1, and why mAP can drop while top-1-equivalent metrics look fine.

**`21_vision_transformers.md` — Quantizing ViTs**
The bridge chapter between Parts III and IV. Post-LayerNorm and post-Softmax distributions and
their long tails. PTQ4ViT's twin uniform quantization. FQ-ViT's Log2 softmax quantization and
power-of-two-factor LayerNorm. RepQ-ViT's scale reparameterization. GELU's asymmetry. Why ViTs sit
between CNNs and LLMs on every axis, which is exactly why chapter 34's argument works.

### Part IV — LLM Quantization

**`22_why_llms_differ.md` — Why LLMs Are a Different Problem**
The roofline argument: decode is memory-bound at batch size 1, so weight-only 4-bit gives near-linear
speedup while doing nothing for prefill. Arithmetic-intensity numbers for prefill vs decode. Outlier
channels (link to ch. 15). Per-token dynamic quantization being cheap here. The KV-cache growing
with context and eventually dominating. Why "quantize the weights, leave the activations alone" is
a perfectly good answer for LLMs and a bad one for CNNs.

**`23_weight_only_baselines.md` — Weight-Only Baselines**
LLM.int8() — the mixed-precision decomposition, the 6.0 outlier threshold, why it is accurate and
slow. bitsandbytes NF4 with double quantization, exactly as QLoRA uses it. HQQ. `optimum-quanto`.
TorchAO's `int4_weight_only` / `int8_weight_only` and the tinygemm layout. These are the
zero-calibration options — establish them as the baseline that GPTQ and AWQ must beat.

**`24_gptq.md` — GPTQ**
Full derivation: Optimal Brain Damage → Optimal Brain Surgeon → OBC → GPTQ. The layer-wise objective,
the Hessian $H = 2XX^\top$ from calibration activations, the closed-form weight update after
quantizing one column, the Cholesky reformulation that makes it stable and fast, lazy batch updates
and why the arbitrary column order works. Then the practical knobs: `group_size`, `desc_act`
(act-order) and its kernel cost, `damp_percent`, symmetric vs asymmetric, the `bits` choice.
GPTQModel/AutoGPTQ and `llm-compressor` usage. Failure modes: calibration-domain sensitivity,
Hessian singularity on dead channels, degradation below 3-bit without small groups.

**`25_awq.md` — AWQ**
The observation: protecting the ~1% of weight channels with the largest corresponding *activation*
magnitudes recovers most of the loss. Why per-channel scaling is mathematically equivalent to a
diagonal similarity transform that leaves the layer output unchanged in float. The grid search over
the scaling exponent. Why AWQ needs no backward pass and no Hessian, and is therefore much faster
than GPTQ. GEMM vs GEMV kernel versions. AutoAWQ and `llm-compressor`. Failure modes and the
models where AWQ and GPTQ disagree.

**`26_smoothquant.md` — SmoothQuant and Activation-Weight Balancing**
$Y = (X \operatorname{diag}(s)^{-1})(\operatorname{diag}(s) W)$ derived, with the per-channel
$s_j = \max|X_j|^\alpha / \max|W_j|^{1-\alpha}$ rule and the meaning of $\alpha$. Why the scaling
folds into the preceding LayerNorm and is therefore free at runtime — and what happens when there
is no preceding LayerNorm to fold into. W8A8 kernels and the actual speedup. The explicit
connection to cross-layer equalization (ch. 10): same idea, different era, different architecture.
When $\alpha$ tuning is required.

**`27_rotation_methods.md` — Rotation and Incoherence**
The insight that an orthogonal transform applied to weights and its inverse to activations leaves
the model exactly invariant while redistributing outlier energy. Computational invariance in
transformers (SliceGPT's framing). QuaRot — randomized Hadamard, fused where possible, online where
not. SpinQuant — learning the rotation with Cayley optimization on the Stiefel manifold. FlatQuant's
learned affine transforms. DuQuant's rotation-plus-permutation. The cost question: which rotations
fold into adjacent weights for free and which must run every forward pass. Why this family
currently defines the state of the art for W4A4.

**`28_w4a8_and_fullstack.md` — W4A8 and Full-Stack Schemes**
QServe/QoQ: progressive group quantization (INT8 intermediate so dequantization stays in the integer
domain), protective range, SmoothAttention, and the W4A8KV4 target. Atom's mixed-precision channel
reordering. ZeroQuant v1/v2/FP-family. OmniQuant's learnable weight clipping and learnable
equivalent transformation. The theme of this chapter: the interesting engineering is in making
*dequantization* cheap, not in the quantizer.

**`29_fp8_and_fp4.md` — FP8 and FP4**
FP8 recipes: per-tensor static, per-tensor dynamic, per-token/per-channel, and the delayed-scaling
amax history used in training. E4M3 for forward, E5M2 for gradients, and why. Transformer Engine.
Then FP4: NVFP4 (block 16, FP8 E4M3 scale, plus a per-tensor FP32 scale) vs MXFP4 (block 32,
E8M0 power-of-two scale) — compare the two directly, since the block size and scale format are the
whole story. Blackwell's native FP4 path. What is known about FP4 quality and where the honest
uncertainty is.

**`30_extreme_low_bit.md` — 2-Bit and Below**
Where uniform quantization stops working and codebooks take over. QuIP#, AQLM, PB-LLM's partially
binarized scheme. BitNet and BitNet b1.58 — ternary weights, `absmean` quantization, why it must be
trained from scratch, and the honest reading of its scaling claims. OneBit. GGUF's TQ1_0/TQ2_0.
A frank section on where quality actually collapses and how to detect it (KL-divergence to the FP
model rather than perplexity — link to ch. 48).

**`31_kv_cache_quantization.md` — KV-Cache Quantization**
KV memory arithmetic first (per-token bytes as a function of layers, heads, head-dim, dtype).
Why keys and values want *different* granularities — key outliers are per-channel, value outliers
are not — as established by KIVI and KVQuant. Per-channel K / per-token V. The residual
full-precision window for recent tokens. FP8 KV as the cheap default on Hopper. Interaction with
PagedAttention and prefix caching. Measured quality impact vs context length.

**`32_quantized_finetuning.md` — Fine-Tuning Quantized Models**
QLoRA: NF4 base, double quantization, paged optimizers, BF16 LoRA adapters, and the gradient path
through a frozen 4-bit base. LoftQ's SVD-based initialization to close the quantization gap.
QA-LoRA's group-wise adaptation that keeps the merged model quantized. EfficientQAT's two-stage
block-wise then end-to-end training. BitDelta. The merge-time trap: merging a BF16 adapter into an
NF4 base and re-quantizing loses much of what you trained.

**`33_moe_ssm_multimodal.md` — MoE, SSMs, and Multimodal**
MoE: per-expert scales, the shared/dense expert needing higher precision, router logits staying in
float, and why expert weights are unusually quantization-friendly (each is rarely used, so error
is diluted — and why that intuition partly fails). SSMs: Mamba's state-space recurrence, the
selective-scan numerics, Quamba. VLMs: the vision tower and the language tower have completely
different sensitivity profiles; quantize them separately. Embedding and reranker models, where
output-vector geometry matters more than any token-level metric.

**`34_cross_pollination.md` — Do LLM Methods Work on CNNs?**
The chapter the reader will come back to. Structure it as an argument, not a list.

*Part 1 — The lineage.* Establish that the direction of travel was originally CNN → LLM:
- GPTQ is OBQ, which is Optimal Brain Surgeon, which was developed and evaluated on ConvNets.
- SmoothQuant's per-channel migration is cross-layer equalization (DFQ, 2019, evaluated on
  MobileNet) applied to a transformer.
- AWQ's salience weighting is sensitivity-weighted PTQ.
- AdaRound and BRECQ were built for CNNs and were later reused wholesale for LLMs.
- Hadamard/incoherence processing is classical decorrelation.
Present this as a timeline component. The conclusion the reader should reach: these are not
"LLM techniques", they are *linear-layer techniques* that got famous on LLMs.

*Part 2 — What actually transfers, mechanically.* For each method, state the requirement it places
on the layer and whether a conv satisfies it:
- Conv as GEMM: `im2col` / unfold turns a conv into a matmul with input dim $C_{in}k_hk_w$, which
  is exactly the axis GPTQ/AWQ group along. Show the reshape.
- Hessian estimation from calibration activations works identically — you just accumulate over
  spatial positions as well as batch, which *helps* (more samples per calibration image).
- Per-group scaling along the input dim: fine for a 3×3 conv with 256 input channels (2304 elements
  per output channel, 18 groups of 128); degenerate for a depthwise conv (9 elements total).
- Equalization needs a scale-equivariant path to fold into. Conv→BN→ReLU→Conv gives you one
  (that's CLE). Conv→Swish does not, cleanly.
- Rotations need a residual stream with a consistent channel basis. Transformers have one; a CNN
  with spatial downsampling between stages does not, though 1×1 convs within a stage can be rotated.

*Part 3 — What does not transfer.* Depthwise convs. Activation outliers being spatial and
input-dependent rather than channel-consistent (link ch. 15), so SmoothQuant's per-channel
migration has less to bite on. Absence of grouped-INT4 conv kernels in every CNN runtime — you can
*compute* the GPTQ solution for a ResNet and then have no hardware that will execute a
group-quantized conv, so the win is compression, not latency. Batch-norm statistics interacting
with weight modification.

*Part 4 — The recipe.* Concrete, runnable: apply GPTQ-style reconstruction to a ViT and to the
1×1 convs of a ResNet using calibration activations, with the unfold, the Hessian accumulation, and
the group assignment spelled out. Report what to expect. Cite the published work that has done
this (ViT-specific PTQ, GPTQ applied outside LLMs, the AWQ authors' own remarks on generality) and
be explicit where the evidence is thin rather than inventing results.

*Part 5 — The decision table.* Method × (CNN / depthwise CNN / ViT / LLM) → transfers cleanly /
transfers with modification / does not transfer, with a one-line reason each.

### Part V — Formats and Serialization

**`35_gguf_part1.md` — GGUF I: Container and Block Quantization**
The container: header, KV-metadata, tensor info, alignment, the `general.*` and architecture keys,
and how a single file carries tokenizer, hyperparameters, and weights. Then the core concept:
**block-wise quantization from first principles.** Why llama.cpp does not use per-channel scales —
the dot-product kernel streams weights sequentially and dequantizes into SIMD registers, so the
scale must be adjacent in memory to the values it scales. Block of 32 as a cache-line and
register-width compromise. Then the legacy types, each with a `.bitfield` layout, the exact struct
from `ggml-common.h`, the dequant formula, and the bytes-per-block arithmetic:
- `Q4_0`: 32 weights, one FP16 scale, symmetric — `y = d * (q - 8)` — 18 bytes / 32 weights
- `Q4_1`: adds an FP16 min — `y = d*q + m`
- `Q5_0`, `Q5_1`: the fifth bit held in a separate 32-bit mask
- `Q8_0`: 32 weights, FP16 scale, symmetric
Include a from-scratch NumPy dequantizer for `Q4_0` in a code fold, verified against a known block.

**`36_gguf_part2.md` — GGUF II: K-Quants, I-Quants, and Choosing**
**K-quants**: the superblock of 256 split into 16 sub-blocks of 16 (or 8 of 32), with sub-block
scales themselves quantized against a superblock-level FP16 scale — two-level quantization. Walk
`Q4_K` byte by byte: 256 weights, 8 sub-blocks, 6-bit scales and 6-bit mins, one FP16 `d` and one
FP16 `dmin`, 144 bytes total → 4.5 bits/weight. Do the same arithmetic for `Q2_K`, `Q3_K`, `Q5_K`,
`Q6_K`, and produce the true effective-bits table rather than the nominal one.
**I-quants**: codebook/lattice-based (`IQ1_S`, `IQ1_M`, `IQ2_XXS`…`IQ4_XS`), what the codebook is,
why they reach lower bits than K-quants at equal quality, and their CPU cost.
**TQ1_0 / TQ2_0**: ternary types for BitNet-class models.
**The importance matrix (`imatrix`)**: what it is (per-channel activation-magnitude statistics from
a calibration run), how `llama-imatrix` produces it, how `llama-quantize` consumes it, which types
require it, and how the calibration corpus choice affects the result. This is GGUF's answer to AWQ,
and the chapter should say so.
**Type mixes**: what `_S` / `_M` / `_L` actually change (per-tensor type assignment — attention vs
FFN vs embeddings vs output), read from `llama-quant.cpp`, not guessed.
Close with the **selection table**: RAM budget × model size × CPU-vs-GPU × quality floor → type,
and an explicit statement of which types are known-bad choices.

**`37_other_formats.md` — Every Other Format You'll Meet**
`safetensors` + `compressed-tensors` (the vLLM/llm-compressor schema: how scales, zero-points, and
the quantization config are stored). GPTQ's packed INT4 layout and `quantize_config.json`. AWQ's
layout and the reorder trick. EXL2/EXL3. ONNX QDQ on disk. TFLite flatbuffers and their
per-axis quantization parameters. TensorRT engines and calibration caches. MLX. CoreML. QNN DLC.
The unifying **master table**: format → scale granularity (tensor / channel / group / block) →
zero-point support → metadata location → who can execute it. This is where the reader sees, in one
view, that "channel-wise vs block-wise" is a format-level decision driven by kernels.

### Part VI — Frameworks and Kernels

**`38_pytorch_quantization.md` — PyTorch Quantization**
The three generations, honestly labeled: eager mode (manual, legacy), FX graph mode (maintenance),
and **PT2E** (`torch.export` + a backend `Quantizer`, the current path). Observers and their
qscheme options mapped onto chapter 6's calibrators. `QConfigMapping`. Writing a custom observer.
QAT modules. TorchAO's `quantize_` API and its affine-quantized-tensor subclass design. What each
path can and cannot express, and the export target each is designed for.

**`39_deployment_toolchains.md` — Vendor and Edge Toolchains**
One subsection each, all following the same template (input format, quantization scheme it enforces,
calibration interface, what it does automatically, gotchas): NVIDIA TensorRT + TensorRT Model
Optimizer; ONNX Runtime; OpenVINO + NNCF; TFLite / AI Edge Torch; Qualcomm QNN & SNPE with AIMET;
Brevitas (+ FINN); Intel Neural Compressor; ARM Ethos-U with Vela and CMSIS-NN; Renesas e-AI / DRP-AI;
Hailo; Apple CoreML / ANE. Close with the capability matrix that feeds Appendix D.

**`40_llm_serving_stacks.md` — Serving Stacks**
vLLM, SGLang, TensorRT-LLM, llama.cpp/Ollama, ExLlamaV2/V3, MLC-LLM, TGI, and the
`llm-compressor` producer. For each: which quantized formats it ingests, which kernels it has,
whether it can quantize on the fly, and where it silently falls back to a slow path. A single
"I have format X and hardware Y — what serves it?" table.

**`41_quantized_kernels.md` — How a Quantized Matmul Actually Runs**
The chapter that explains why quantization sometimes doesn't speed anything up. Integer tensor-core
paths and `dp4a`. Mixed-input GEMM: W4A16 requires dequantizing weights into registers before the
FP16 MMA, so the win is bandwidth, not math. Marlin and Machete as the state of the art for that.
Dequant-fused GEMV for batch-1 decode. CUTLASS mixed-input support. Arithmetic-intensity arithmetic:
compute the crossover batch size at which W4A16 stops helping. Why W8A8 and W4A4 are the only
schemes that reduce FLOPs, and what they cost in accuracy to get there.

### Part VII — Practice

Hands-on chapters. Each is a complete, runnable narrative with checkpoints where the reader
verifies a number. Long listings go in code folds.

**`42_handson_cnn_ptq.md` — Quantizing a CNN End to End**
ResNet-18 and MobileNetV2 on a small eval set. Implement min-max, percentile, and KL calibrators
from scratch against the theory in chapter 6. Per-channel weight quantization. Measure top-1 and
per-layer SQNR. Show MobileNetV2 failing with per-tensor weights and being rescued by per-channel —
the pedagogically essential experiment.

**`43_handson_export_and_deploy.md` — Export and Deploy**
PT2E quantization → ONNX with Q/DQ nodes → ONNX Runtime, and → TensorRT INT8. Inspect the exported
graph to *prove* the integer kernels are being used (Netron, TensorRT verbose logs, profiler output),
because "it exported" and "it runs in INT8" are different claims.

**`44_handson_qat.md` — QAT From Scratch**
An STE fake-quant autograd function, then an LSQ quantizer with the learned step size and gradient
scale, both implemented directly. Train, compare against the PTQ baseline from chapter 42, then
swap in `torch.ao` / TorchAO's implementations and confirm agreement.

**`45_handson_llm.md` — Quantizing an LLM**
One small model, four treatments: GPTQ, AWQ, SmoothQuant W8A8, and FP8 — via `llm-compressor` and
the native libraries. Serve each on vLLM. Measure perplexity, KL-divergence to the FP16 model, and
decode throughput. Produce the comparison table the reader can rerun.

**`46_handson_gguf.md` — GGUF From Conversion to Benchmark**
`convert_hf_to_gguf.py` → `llama-imatrix` → `llama-quantize` across several types →
`llama-perplexity` and `llama-bench`. Show the file-size and quality curve, with and without an
imatrix, and read one block out of the resulting file with the NumPy dequantizer from chapter 35 to
confirm the layout is what the chapter claimed.

**`47_debugging_playbook.md` — The Debugging Playbook**
Symptom-driven, not method-driven. For each symptom: the likely causes ranked, the diagnostic that
distinguishes them, and the fix.
- Accuracy collapses to chance → scale/zero-point sign or dtype bug, not a quantization problem
- Small but unacceptable drop → sensitivity sweep, then selective fallback
- Works in the framework, breaks after export → operator semantics mismatch; how to bisect
- Fine on the calibration set, bad in production → calibration domain mismatch
- Only some inputs break → outlier-driven clipping
- Quantized but not faster → no integer kernel; check the profiler
Tooling: per-layer SQNR and cosine-similarity harnesses, activation histogram dumps, bisection by
progressively quantizing prefixes of the network, numerical comparison across runtimes.

**`48_evaluating_quantized_models.md` — Evaluating Honestly**
Why perplexity is a weak signal for quantization damage and **KL-divergence to the full-precision
model's output distribution** is a much better one. Downstream task evals and their variance.
mAP/IoU for vision. Latency, TTFT, ITL, throughput at a given batch size, and energy. The memory
formula (weights + KV + activations + overhead) worked for real configurations. A checklist for
reporting a quantization result that someone else could reproduce — and the common ways published
comparisons mislead.

**`49_decision_guide.md` — The Decision Guide**
Synthesis. Flowcharts by starting point: "I have an NPU and a CNN", "I have one A100 and a 70B
model", "I have a laptop", "I have a latency SLA", "I have no calibration data". Each terminates in
a concrete recipe with a link to the chapter that explains it. This chapter contains no new
material — it is the index into everything before it.

### Appendices

- **`appendix_a_glossary.md`** — every term and symbol, one canonical definition each, alphabetical.
- **`appendix_b_math.md`** — the derivations pulled out of the chapters so they can be found:
  MSE-optimal scale, optimal clipping threshold under Gaussian and Laplacian assumptions, the KL
  calibration objective, the OBS update, the STE gradient and its bias, error propagation through a
  linear layer, effective-bits accounting.
- **`appendix_c_method_index.md`** — the master table. One row per method: name, year, authors,
  class, target, bits, calibration requirement, granularity, hardware, tooling, paper link. This is
  the table every method chapter must agree with.
- **`appendix_d_hardware_matrix.md`** — accelerator × supported precisions × granularity limits ×
  accumulator width × toolchain × notes. Sourced from vendor documentation, with the doc linked.
- **`appendix_e_bibliography.md`** — the full reading list grouped by theme, with a one-line note on
  why each paper matters and a suggested reading order for someone entering the field.

---

## 7. Per-chapter completion checklist

A chapter is not done until every box is true.

- [ ] Front-matter present, `title:` matches the `# ` heading exactly
- [ ] `[← Back to Table of Contents](./README.md)` is the first line of the body
- [ ] Epigraph present and specific to this chapter
- [ ] `## Overview` with a diagram
- [ ] Notation matches §3.1 with no substitutions
- [ ] Every named method has the §3.2 fact block
- [ ] Every number traces to a row in the References table, or is explicitly marked an estimate
- [ ] Every derivation ends with a numerical check
- [ ] Code snippets over 25 lines are in a `<details class="code-fold">` pane with a length in the summary
- [ ] No HTML class outside the §4.2 vocabulary
- [ ] No blank lines inside an HTML component block
- [ ] Comparison tables have a "Choose this when" column
- [ ] All ten questions in §5 answered for each method
- [ ] `## Key Takeaways` present, claims not summaries
- [ ] `## References` table filled
- [ ] `**Next:**` footer links to the correct next file
- [ ] Internal links use `./NN_slug.md` and resolve to files that exist
- [ ] `appendix_c_method_index.md` updated with any methods introduced
- [ ] Chapter listed in `_config.yml` `navigation:` and in the `README.md` TOC
- [ ] Read the chapter start to finish once, out of context, and cut anything that does not teach

---

## 8. Working order

Write in this order — later chapters depend on earlier ones for notation and cross-links:

1. **Part I (01–07)** first. Everything else references it.
2. **Part II (08–16)**, which establishes the method vocabulary.
3. **Appendix C** skeleton, so method chapters have somewhere to record rows.
4. **Parts III and IV (17–34)**, the method chapters.
5. **Part V (35–37)**, which needs Part IV's methods to compare against.
6. **Part VI (38–41)**.
7. **Part VII (42–49)** last — the hands-on and synthesis chapters cite everything.
8. **Chapter 00** and the remaining appendices at the very end, once the material is stable.
9. Final pass: notation audit, link audit, Appendix C reconciliation, README TOC.
