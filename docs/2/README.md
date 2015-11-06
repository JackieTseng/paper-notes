## Deep Compression: Compressing Deep Neural Network with Pruning, Trained Quantization and Huffman Coding -- Paper Note
### Introduction

Compressing the neural networks by a three-stage pipeline: pruning, trained quantization and huffman coding to reduce the storage requirement. This allows fitting the model into mobile apps given size limit.

### Problem

1. **Computationally intensive** and **memory intensive**, making them difficult to deploy on embedded systems with limited hardware resources.
2. **Large storage overhead** prevents deep neural networks from being incorporated into mobile apps.
3. **Energy consumption**

### Method

1. Prunes the network by learning only the important conncetions and removing the redundant connections, keeping only the most informative connections.
2. Quantize the weights to enforce weights sharing and apply Huffman coding.
3. Retrain the network to fine tune the remaining connections and the quantized centroids.

### Achievement

On the ImageNet dataset, reduced the storage of AlexNet by **35X**, from **240** MB to **6.9** MB while VGG-16 network can be reduced **49X**, from **552** MB to **11.3** MB. All with no loss of accuracy.

### Detailed Process

### Experiment
