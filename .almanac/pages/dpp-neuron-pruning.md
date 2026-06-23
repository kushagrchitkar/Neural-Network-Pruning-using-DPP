---
title: DPP Neuron Pruning
summary: The project prunes whole hidden neurons by sampling high-activation, low-redundancy subsets with k-DPPs and retraining the resulting condensed MLP.
topics: [pruning, dpp, experiment]
sources:
  - id: repo-readme
    type: file
    path: README.md
    note: States the project goal, DPP-based method, and claimed pruning efficiency.
  - id: notebook
    type: file
    path: MLP Implementation/Pruning_Implementation_MLP.ipynb
    note: Implements activation collection, DPP kernels, neuron selection, pruning, and retraining.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: Explains the thesis motivation and intended pruning procedure.
status: active
verified: 2026-06-23
---

DPP neuron pruning is the central project idea: choose entire hidden neurons from a trained dense MLP instead of pruning individual weights. The project frames this as a practical response to Lottery Ticket-style sparse masks, because a sparse matrix can still leave GPU computation shaped like the original matrix, while removing neurons changes the layer dimensions themselves [@slides].

The pruning criterion combines two properties. A neuron should have high contribution, represented by its average activation over the Fashion-MNIST training set, and it should be diverse from the other retained neurons, represented by pairwise activation correlation. The notebook turns those two properties into one DPP likelihood kernel per hidden layer and samples a fixed-size subset of neuron indexes from each kernel [@notebook]. See [[dpp-kernel-contract]] for the exact kernel contract.

The implementation applies DPP independently to the two hidden layers of the baseline 784-300-100-10 MLP. With pruning ratio `z = 0.75`, the retained hidden widths are `int((1 - z) * 300) = 75` and `int((1 - z) * 100) = 25`, giving a condensed 784-75-25-10 architecture [@notebook]. The slide deck describes that same 75% pruning path as: apply DPP on each layer, test the pruned network, then retrain using the retained weights [@slides].

The project's durable conclusion is not that raw pruning alone is sufficient. The presentation explicitly says simply pruning causes a large accuracy drop, while retraining restores accuracy near the initial model [@slides]. The README records the same thesis-level claim: the original model reaches about 87.19%, the pruned untrained model about 66.93%, and the pruned-and-retrained model about 87.9% [@repo-readme]. The checked-in notebook output differs in some places, so treat [[fashion-mnist-mlp-experiment]] as the page that reconciles reported results with executable evidence.

The current implementation is research-notebook code, not a reusable pruning library. There is no packaged API, command-line entrypoint, deterministic seed control, or automated test that enforces the DPP behavior [@notebook]. Future implementation work should first decide whether the project is preserving a thesis artifact or turning the method into a reproducible experiment pipeline; those goals imply different fixes.

Related pages: [[fashion-mnist-mlp-experiment]], [[dpp-kernel-contract]], [[notebook-workflow]], [[model-artifacts]], [[lottery-ticket-context]].

