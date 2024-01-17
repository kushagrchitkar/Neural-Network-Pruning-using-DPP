# Neural Network Pruning with Determinantal Point Process Algorithm

## Overview

The development of larger neural networks has been crucial to the advancement of deep learning. However, the challenges associated with distributing these models, such as increased storage requirements and computational expenses, have prompted the need for efficient pruning techniques. Pruning involves eliminating extraneous neurons or weights from a neural network.

We have developed a novel neural network pruning algorithm based on Determinantal Point Process. This README provides an overview of our algorithm's application to a fully connected Multi Layer Perceptron (MLP) network using the F-MNIST Dataset.

## Experimentation Details

### Model Architecture
- Fully connected Multi Layer Perceptron (MLP)

### Dataset
- F-MNIST (Fashion MNIST) Dataset

### Pruning Method
- Determinantal Point Process-based pruning

## Experiment Results

The results of our experiments is as follows:
1. **Original Model**
   - Accuracy: 87.19% (8705/9984)

2. **Pruned (Untrained) Model**
   - Accuracy: 66.93% (6683/9984)

3. **Pruned and Retrained Model**
   - Accuracy: 87.9% (8878/9984)

## Pruning Efficiency

- **75% Pruned Network**
  - Achieved accuracy of 87.9% after minimal retraining.
  
- **Further Pruning**
  - Successfully pruned up to 98% of the network with no noticeable drop in accuracy.

## Model Verification

All the models, including the original, pruned (untrained), and pruned and retrained, have been uploaded for verification.

## Conclusion

Our Determinantal Point Process-based pruning algorithm demonstrates efficiency in reducing the size of neural networks without significant loss of accuracy. The experiments showcase the ability to prune up to 98% of the network while maintaining satisfactory performance. For detailed implementation and results, refer to the uploaded models and associated documentation.
