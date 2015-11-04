## Learning both Weights and Connections for Efficient Neural Networks -- Paper Note
### Introduction

Pruning the redundant/unimportant connections using a three-step method in order to reduce the parameters, storage and computations of neural networks and applied to the mobile devices.

### Problem

1. Significant redundancy for deep learning models.
 + The relative approximation and quantization techniques are orthgonal to network pruning, and can potentially be used together to obtain further gains.
2. Large number of parameters of nerual networks.
3. Complexity and Over-fitting.

### Method

1. Train the network to learn which connections are important.
2. Prune the unimportant connections.
3. Retrain the network to fine tune the weights of the remaining connections.

### Achievement

On the ImageNet dataset, reduced the number of parameters of AlexNet by a factor of **9X**, from **61** million to **6.7** million while VGG16 network can be reduced **13X**. All with no loss of accuracy.

### Detailed Process

1. Train the whole network to learn which are important connections rather than the final values of the weights.
2. Prune the low-weight connections which weights below a set threshold -- converting a dense network into a sparse network.
3. Retrain the network to learn the final weights for remaining sparse connections. This step is critical for the accuracy and network performance. Restart from Step 2 iteratively untill the network is stable.

<p align="center">
<img src="img/process.png"/>
</p>

+ **Regularization** : Choose the correct regularization.
 + **L1 regularization** penalizes no-zero parameters resulting in more parameters near zero. This gives better accuracy after pruning, but before retraining.
 + **L2 regulatization** will result in lower accuracy after retraining but give the best pruning results.

+ **Dropout and Capacity Control**
 + Dropout can prevent over-fitting and applies to retraining. However, this mothod is regarded as ***soft dropout***, since each parameter is not definitely dropped out. The method mentioned in this paper is ***hard dropout***.
 + As the parameters get sparse, the classifier will select the most informative predictors and thus have much less prediction variance which reduces ***over-fitting***.
 + As pruning already reduced model capacity, the retraining dropout ratio should be smaller. As follow :

<p align="center">
<img src="img/dropout_ratio.png" alt="Dropout Ratio"/>
</p>

+ **Local Pruning and Parameter Co-adaptation**
 + During retraining, it is better to retain the weights from the initial training phase for the connections that survived pruning than it is to re-initialize the pruned layers since CNNs contain ***fragile co-adapted*** features.
 + Dealing with the ***vanishing gradient problem***, the author fix the paramters for part of network and only retain a shallow network by reusing the surviving parameters, with already co-adapted well with the un-pruned layers during initial training.

+ **Iteraitve Pruning**
 + Pruning followed by a retraining is one iteration, after many such iterations the minimum number conncetions could be found.

+ **Pruning Neurons**
 + After pruning connections, neurons with zero input conncetions or zero output connectiongs may be safely pruned.
 
+ **Pruning Threshold** 
 + Chosen as a quality parameter multiplied by the standard deviation of a layer's weights.

### Experiment

* L1 regularization performs better than L2 at learning the connectiongs without retraining, while L2 regularization performs better than L1 at retraining. **Iterative pruning** gives the best result.
* Both **CONV** and **FC** layers can be pruned, but with different sensitivity.
* Using the sensitivity results to find each layer's threshold, like the smallest threshold is applied to the most sensitive layer.
* Storing the pruned layers as sparse matrives.
* Storing relative rather than absolute indices reduces the space taken by the **FC** layer indices to 5 bits. Similarly, **CONV** layer indices can be represented with only 8 bits.

### Q&A

1. Does important means that the weight is small than the threshold?
2. How to compute the proper threshold?
3. Are soft dropout and hard dropout used at the same time? 
4. How to set dropout rate?
5. When will I stop training or retraining phase and step into pruning phase?
6. When is the network stable for finishing process?
7. I don't understand the relative and absolute indices.
8. How can a weight and neuron be deleted in Caffe? 
