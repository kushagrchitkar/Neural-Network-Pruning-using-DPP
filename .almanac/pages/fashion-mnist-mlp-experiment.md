---
title: Fashion-MNIST MLP Experiment
summary: The experiment trains a 784-300-100-10 Fashion-MNIST MLP, prunes hidden neurons with DPP, and retrains a 784-75-25-10 model for comparison.
topics: [experiment, fashion-mnist, mlp, pruning]
sources:
  - id: repo-readme
    type: file
    path: README.md
    note: Summarizes architecture, dataset, method, and headline results.
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Contains model definition, data loaders, training loop, pruning code, and captured outputs.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: Reports presentation metrics and experiment framing.
status: active
verified: 2026-06-23
---

The experiment uses Fashion-MNIST because the thesis considered classic MNIST too easy for the comparison: standard networks already exceed 90% accuracy, making pruning effects less informative [@notebook]. Fashion-MNIST supplies 60,000 training images and 10,000 test images of 28-by-28 apparel classes; the notebook uses `torchvision.datasets.FashionMNIST` with tensor conversion and normalization around mean/std `0.5` [@notebook].

The baseline network is a fully connected MLP:

| Layer | Shape |
| --- | --- |
| input | flattened 28 x 28 image, 784 features |
| `fc1` | 784 -> 300 |
| `fc2` | 300 -> 100 |
| `fc3` | 100 -> 10 |

Both hidden layers use ReLU, and the output uses `F.log_softmax(dim=1)` for `nn.NLLLoss` [@notebook]. Training uses Adam with learning rate `0.0003`, weight decay `1e-4`, batch size `32`, and 50 epochs [@notebook]. The slides state the same architecture, optimizer, epoch count, and learning rate [@slides].

The 75% pruning experiment changes hidden widths from 300 and 100 to 75 and 25. The notebook uses `z = 0.75`, computes DPP-selected indexes per hidden layer, extracts matching weight rows and columns, and defines a second `FeedForwardNet` class with `h1 = int((1 - z) * 300)` and `h2 = int((1 - z) * 100)` [@notebook].

The result record has two layers of evidence:

| Source | Initial model | Pruned, not retrained | Pruned and retrained |
| --- | --- | --- | --- |
| README | 8705/9984, 87.19% | 6683/9984, 66.93% | 8878/9984, 87.9% |
| slides | 8705/9984, 87.19% | 6683/9984, 66.93% | 8778/9984, 87.9% |
| notebook captured outputs | 8897/9984 | 4184/9984 | 8755/9984 |

The README and slides appear to preserve the thesis presentation result, while the notebook outputs preserve one concrete executed run with different stochastic outcomes and possible state issues [@repo-readme] [@slides] [@notebook]. Future agents should not collapse these into one number without rerunning under controlled seeds.

The test denominator is `9984`, not 10,000, because the test loader uses `batch_size=32`, `drop_last=True`, and Fashion-MNIST's 10,000 test examples leave a final incomplete batch of 16 examples that is dropped [@notebook]. This matters when comparing reported accuracies or reproducing counts.

Related pages: [[dpp-neuron-pruning]], [[dpp-kernel-contract]], [[notebook-workflow]], [[model-artifacts]], [[reproducibility-and-environment]].

