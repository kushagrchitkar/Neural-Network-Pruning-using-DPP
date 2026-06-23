---
title: Reproducibility And Environment
summary: Reproducing the experiment requires reconstructing an old PyTorch notebook environment, controlling randomness, and fixing CPU/GPU and stateful-name assumptions.
topics: [reproducibility, environment, pytorch]
sources:
  - id: requirements
    type: file
    path: requirements.txt
    note: Lists unpinned and pinned Python dependencies.
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Shows the runtime assumptions, data path, stochastic calls, and notebook bugs.
  - id: file-listing
    type: file
    path: MLP Implementation/
    note: Contains model artifacts that may be used to verify reruns.
status: active
verified: 2026-06-23
---

The repo is a notebook-era research artifact, not a fully reproducible experiment package. `requirements.txt` lists the dependency families, but several core packages are unpinned: `matplotlib`, `numpy`, `Pillow`, `scipy`, `torch`, `torchvision`, and `dppy` [@requirements]. Pinned packages such as `pandas==1.2`, `seaborn==0.9.0`, and `tqdm==4.36.1` suggest an older Python ecosystem, but the repo does not declare a Python version [@requirements].

The notebook depends on:

- `torch` and `torchvision` for model training and Fashion-MNIST loading.
- `dppy` for `FiniteDPP` sampling.
- `scipy.stats` for Pearson correlation.
- `numpy` for activation matrices and kernel construction.
- `matplotlib` for loss plots.
- CUDA availability in the captured run [@notebook].

Data is downloaded or read from `~/.pytorch/F_MNIST_data` through `torchvision.datasets.FashionMNIST` [@notebook]. The repo does not include the dataset, so reruns need network access for the first dataset download unless the local cache already exists.

Randomness is uncontrolled. The notebook does not set seeds for PyTorch, NumPy, CUDA, DataLoader shuffling, or DPP sampling [@notebook]. That explains why the README/slides and captured notebook outputs should be treated as related evidence rather than one exact reproducible metric set. Any future benchmark claim should include the seed, library versions, hardware, and whether the model is initialized from retained weights or from scratch.

CPU-only reruns need a code fix before evaluation. The helper `is_gpu` is called incorrectly in test loops, so `.cuda()` is always attempted [@notebook]. Clean reruns also need the final test cell to use `retrained_model`, not the undeclared `modell` name [@notebook].

Saved full-module PyTorch artifacts are fragile across environments. `initial_model.pth` and `final_model.pth` were saved as complete module objects, not `state_dict`s [@notebook]. A more stable archive would include model class definitions, `state_dict`s, architecture metadata, pruning ratio, selected neuron indexes, training metrics, and environment lockfiles.

Related pages: [[notebook-workflow]], [[model-artifacts]], [[fashion-mnist-mlp-experiment]], [[dpp-kernel-contract]].

