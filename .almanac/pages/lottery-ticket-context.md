---
title: Lottery Ticket Context
summary: The project positions DPP neuron pruning as a whole-neuron alternative to Lottery Ticket-style sparse weight pruning for more directly condensed computation.
topics: [research-context, pruning, lottery-ticket]
sources:
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Contains the project motivation written in the opening markdown cell.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: Frames the proposal against Lottery Ticket Hypothesis methodology.
  - id: repo-readme
    type: file
    path: README.md
    note: Summarizes the project as a DPP pruning method for an MLP on Fashion-MNIST.
status: active
verified: 2026-06-23
---

The project's research context is the Lottery Ticket Hypothesis, especially the idea that subnetworks can retain strong performance after pruning. The notebook opening says MIT researchers showed such subnetworks exist by pruning specific weights, but argues that sparse weight masks have limited practical value for GPU algebra because the matrix being fed to the GPU can remain roughly the same size [@notebook]. The slides make the same contrast: Lottery Ticket prunes weight parameters, while this project proposes pruning entire neurons [@slides].

The local proposal is not just "prune more." It changes the unit of pruning from individual weights to neurons, then uses k-DPPs to select neurons that are both useful and non-redundant [@slides]. This makes [[dpp-neuron-pruning]] a structural-pruning experiment: after selection, the new architecture is actually smaller, such as 784-75-25-10 for a 75% hidden-neuron pruning run [@slides].

The project also differs from the Lottery Ticket reset-and-train framing. The slides say the method prunes and retrains the condensed model instead of repeatedly reinitializing and pruning until convergence [@slides]. The notebook's current retraining implementation appears to train the smaller architecture without calling its retained-weight initializer, so the exact relationship between "retained weights" and "retrained condensed model" remains a code-level issue for [[notebook-workflow]] [@notebook].

The reusable project belief is that neuron-level pruning is worth investigating because it can change matrix dimensions, not just sparsity patterns. The current repo demonstrates that belief on one Fashion-MNIST MLP and leaves open whether the same DPP kernel, training-time savings, and accuracy behavior transfer to other architectures or datasets [@slides].

Related pages: [[dpp-neuron-pruning]], [[fashion-mnist-mlp-experiment]], [[dpp-kernel-contract]].

