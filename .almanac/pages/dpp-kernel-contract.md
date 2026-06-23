---
title: DPP Kernel Contract
summary: The DPP kernel encodes each hidden neuron by average activation quality and Pearson-correlation diversity before sampling retained neuron indexes.
topics: [dpp, pruning, contracts]
sources:
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Implements the per-layer kernel construction and k-DPP sampling.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: States the intended quality and correlation terms for the kernel.
status: active
verified: 2026-06-23
---

The DPP kernel is the contract between trained network activations and neuron selection. For each hidden layer, the notebook creates an `n x n` likelihood kernel `L`, where `n` is the number of neurons in that layer. Each entry is computed as `L[i][j] = q_i * C_ij * q_j`, with `q` as average activation quality and `C_ij` as the Pearson correlation between the activation vectors for neurons `i` and `j` [@notebook]. The slides describe the same formula and name Pearson correlation as the diversity term [@slides].

The activation vectors come from the full 60,000-sample Fashion-MNIST training set. The notebook collects `FEATS1` with shape `(60000, 300)` after `fc1` and ReLU, and `FEATS2` with shape `(60000, 100)` after `fc2` and ReLU [@notebook]. Average activation is computed column-wise as `av_layer1 = FEATS1.mean(0)` and `av_layer2 = FEATS2.mean(0)` [@notebook].

The DPP library boundary is `dppy.finite_dpps.FiniteDPP`. The notebook instantiates `FiniteDPP('likelihood', **{'L': kernel})` and calls `sample_exact_k_dpp(size=...)` for each hidden layer [@notebook]. At `z = 0.75`, the first call samples 75 indexes from the 300-neuron layer and the second samples 25 indexes from the 100-neuron layer [@notebook].

There are three important implementation assumptions:

- Correlations that return `NaN` are converted to `0`. This handles constant activation vectors but also silently removes those pairwise relationships from the kernel [@notebook].
- The kernel uses raw Pearson correlation, not an explicit similarity transform constrained to nonnegative values. Future work that formalizes the method should verify that the resulting likelihood kernel satisfies DPP requirements for the chosen `dppy` version.
- The DPP sampling is stochastic unless the random state is controlled. The notebook does not set a NumPy, PyTorch, or DPP seed [@notebook].

The contract is layer-local. The selected first-layer indexes are also used to remove the corresponding input columns from the second layer's weight matrix before selecting second-layer rows, because a smaller `fc2` must accept only the retained first-layer outputs [@notebook]. That cross-layer weight reshaping is part of [[notebook-workflow]], not part of the DPP kernel itself.

Related pages: [[dpp-neuron-pruning]], [[fashion-mnist-mlp-experiment]], [[reproducibility-and-environment]].

