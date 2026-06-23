---
title: Thesis And Presentation
summary: The thesis materials frame the repository as a bachelor's thesis project named "More Than a Lottery" about DPP-based neuron pruning.
topics: [research-artifacts, thesis, presentation]
sources:
  - id: thesis-pdf
    type: file
    path: Thesis.pdf
    note: Repository thesis artifact; text was not extracted in this build environment.
  - id: slides
    type: file
    path: Thesis Presentation.pptx
    note: Provides extractable title, author, supervisor, experiment narrative, results, and future work.
  - id: repo-readme
    type: file
    path: README.md
    note: Public summary of the thesis project and model verification claim.
status: active
verified: 2026-06-23
---

The repository is organized around a bachelor's thesis project. The presentation title is "More Than a Lottery: Retraining the Lottery Ticket Hypothesis," presented by Kushagra Chitkara under Prof. Jiaul Hoque Paik [@slides]. The README names the implementation as "Neural Network Pruning with Determinantal Point Process Algorithm" and describes a novel DPP-based pruning algorithm applied to a fully connected MLP on Fashion-MNIST [@repo-readme].

The slide deck is the clearest narrative source in the repo. It introduces Lottery Ticket Hypothesis, states the problem with weight-level pruning for GPU-efficient computation, proposes whole-neuron pruning with k-DPP, defines the baseline Fashion-MNIST MLP, explains the quality/diversity kernel, reports baseline/pruned/retrained results, and lists future work [@slides].

The checked-in [[Thesis.pdf]] is a primary research artifact, but the build environment did not include a PDF text extraction tool or PyPDF dependency. Claims in this wiki are therefore grounded in sources that were readable locally: [[README.md]], [[Thesis Presentation.pptx]], [[MLP Implementation/Pruning_Implementation_MLP.ipynb]], and [[requirements.txt]]. Future agents with PDF extraction available should verify whether the thesis adds details not present in the slides.

The presentation's future-work list is still useful for project direction:

- Check whether training time is reduced.
- Experiment with the DPP kernel.
- Try other neural network types.
- Try other datasets [@slides].

Those directions map directly to current repo gaps: no timing benchmark, one hand-written kernel, one MLP architecture, one dataset, and no automated reproduction pipeline.

Related pages: [[lottery-ticket-context]], [[dpp-neuron-pruning]], [[fashion-mnist-mlp-experiment]], [[reproducibility-and-environment]].
