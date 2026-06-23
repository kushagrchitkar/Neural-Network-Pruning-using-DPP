---
title: Notebook Workflow
summary: The only executable implementation is a stateful notebook that trains, samples DPP neuron subsets, mutates weights, defines a smaller model, and saves full PyTorch modules.
topics: [notebook, workflow, reproducibility]
sources:
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Contains the sequential cells and captured outputs that define the workflow.
  - id: requirements
    type: file
    path: requirements.txt
    note: Lists the Python package dependencies expected by the notebook.
status: active
verified: 2026-06-23
---

[[MLP Implementation/Pruning_Implementation_MLP.ipynb]] is the project's executable source of truth. There is no separate Python package, script, test suite, or CLI entrypoint. The notebook is therefore both implementation and experiment log [@notebook].

The intended cell sequence is:

1. Import PyTorch, torchvision, NumPy, SciPy, Matplotlib, and `FiniteDPP`.
2. Load Fashion-MNIST train/test splits from `~/.pytorch/F_MNIST_data`.
3. Define and train the baseline `FeedForwardNet` for 50 epochs.
4. Test the baseline model.
5. Collect hidden-layer activation matrices across the full training set.
6. Build one DPP likelihood kernel per hidden layer.
7. Sample retained neuron indexes with exact k-DPP sampling.
8. Zero the pruned rows in the original dense model and test that mutated model.
9. Build a smaller `FeedForwardNet` whose hidden widths are derived from `z`.
10. Train and save the smaller model.

Two notebook details affect correctness when rerunning or refactoring.

The `is_gpu` helper is defined as a function, but test loops use `if is_gpu:` instead of `if is_gpu():` [@notebook]. In Python, the function object is truthy, so those branches always execute and call `.cuda()`. The captured outputs show the notebook was run on CUDA, so this did not fail in that environment [@notebook]. On a CPU-only environment this bug will make evaluation fail even if training selected `device = "cpu"`.

The retrained model class defines `init_weights`, but the notebook never calls it before training the smaller model [@notebook]. The thesis narrative says the retained weights should be used for the new network, but the captured code trains `retrained_model` after constructing it with default linear-layer initialization. A future cleanup should decide whether to implement retained-weight initialization or update the narrative to say the condensed architecture is retrained from scratch.

The final evaluation cell calls `modell(data)` rather than `retrained_model(data)` [@notebook]. Captured outputs exist for that cell, which means `modell` was defined somewhere in the interactive kernel state outside the visible notebook sequence or from an earlier execution. A clean rerun from top to bottom should not rely on that name.

Related pages: [[fashion-mnist-mlp-experiment]], [[dpp-kernel-contract]], [[model-artifacts]], [[reproducibility-and-environment]].
