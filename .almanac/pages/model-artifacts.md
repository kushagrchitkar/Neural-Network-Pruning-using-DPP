---
title: Model Artifacts
summary: The repository stores a baseline model, a final pruned/retrained model, and additional pruning-ratio checkpoints whose meaning depends on notebook state and PyTorch serialization.
topics: [artifacts, pytorch, pruning]
sources:
  - id: file-listing
    type: file
    path: MLP Implementation/
    note: Contains the saved model files and pruning-ratio checkpoints.
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Shows `torch.save` calls for `initial_model.pth` and `final_model.pth`.
  - id: repo-readme
    type: file
    path: README.md
    note: States that uploaded models are meant for verification.
status: active
verified: 2026-06-23
---

[[MLP Implementation/]] contains the saved experiment artifacts. The README says the original, pruned, and retrained models were uploaded for verification [@repo-readme]. The files currently present are:

| File | Size | Visible role |
| --- | ---: | --- |
| `initial_model.pth` | 1,070,468 bytes | Baseline dense MLP saved by the notebook |
| `final_model.pth` | 248,112 bytes | Final smaller model saved by the notebook |
| `model_0.75` | 249,853 bytes | Historical pruning-ratio checkpoint |
| `model_0.80.pth` | 196,333 bytes | Historical pruning-ratio checkpoint |
| `model_0.85.pth` | 150,413 bytes | Historical pruning-ratio checkpoint |
| `model_0.90.pth` | 98,253 bytes | Historical pruning-ratio checkpoint |
| `model_0.95.pth` | 53,372 bytes | Historical pruning-ratio checkpoint |
| `model_0.97.pth` | 34,252 bytes | Historical pruning-ratio checkpoint |
| `model_0.98.pth` | 24,728 bytes | Historical pruning-ratio checkpoint |
| `model_0.99.pth` | 15,228 bytes | Historical pruning-ratio checkpoint |

The notebook explicitly saves complete module objects with `torch.save(feed_forward_net, "initial_model.pth")` and `torch.save(retrained_model, "final_model.pth")` [@notebook]. These are not plain `state_dict` files. Loading them requires PyTorch and a compatible `FeedForwardNet` class name in the loading environment, because the pickle metadata references `__main__.FeedForwardNet`.

The decreasing sizes of `model_0.80.pth` through `model_0.99.pth` match the interpretation that they are increasingly pruned models, but the notebook does not create those filenames in its visible cells [@notebook]. Treat those files as artifacts from additional runs, not as outputs reproducible from the checked-in notebook without modification.

The `0.75` checkpoint has no `.pth` extension, but its size is close to `final_model.pth`. Do not infer different semantics from the missing extension alone. The reliable naming signal is that `final_model.pth` is the only final artifact saved by the visible notebook.

Related pages: [[fashion-mnist-mlp-experiment]], [[notebook-workflow]], [[reproducibility-and-environment]].
