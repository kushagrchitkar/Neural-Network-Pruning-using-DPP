---
title: Getting Started
summary: Start here to understand the DPP neuron-pruning project, its experiment artifacts, and the wiki pages worth reading first.
topics: [navigation, project-memory]
sources:
  - id: repo-readme
    type: file
    path: README.md
    note: States the project purpose, dataset, method, and headline results.
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Contains the executable experiment workflow and implementation details.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: Provides the thesis narrative, reported values, and future-work framing.
status: active
verified: 2026-06-23
---

This repository is a research artifact for pruning a Fashion-MNIST multilayer perceptron by selecting whole neurons with a Determinantal Point Process. The project memory starts with [[dpp-neuron-pruning]] because that page explains the core idea: preserve neurons that are both high-quality and diverse, then retrain a condensed model rather than rely on sparse weight masks.

Read [[fashion-mnist-mlp-experiment]] next for the experimental setup: dataset, baseline architecture, training loop, pruning ratio, and the reported before/after results. Read [[notebook-workflow]] when changing or rerunning the notebook, because the notebook is the only executable implementation and it contains stateful assumptions that are easy to miss. Read [[model-artifacts]] before treating any `.pth` file as canonical, because the saved artifacts mix full pickled PyTorch modules with historical pruning checkpoints.

For reproducibility work, start with [[reproducibility-and-environment]]. It records the dependency surface, the absence of a script or pinned Python runtime, and the local issues that affect reruns. For research context, read [[lottery-ticket-context]] and [[dpp-kernel-contract]] together: the former explains why this project contrasts itself with weight-level Lottery Ticket pruning, and the latter explains the kernel contract that turns activations into DPP-selected neuron indexes.

The most important local sources are:

- [[README.md]] for the public summary and headline claims.
- [[MLP Implementation/Pruning_Implementation_MLP.ipynb]] for the actual workflow.
- [[Thesis Presentation.pptx]] for the cleaner thesis narrative and reported presentation metrics.
- [[requirements.txt]] for the dependency list.
- [[MLP Implementation/]] for saved model artifacts.

