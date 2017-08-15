---
layout: post
title: Distance Metric Learning using Graph Convolutional Networks Application to Functional Brain Networks
---

Paper: [arxiv](https://arxiv.org/abs/1703.02161)  
Code: [github](https://github.com/sk1712/gcn_metric_learning) (tensorflow)
Submitted: 7 Mar 2017

### Key Idea:
> we propose a novel method for learning a similarity metric between irregular graphs with known node correspondences.

### Backgroung knowledge:
[Siamese neural networks](https://www.quora.com/What-are-Siamese-neural-networks-what-applications-are-they-good-for-and-why) 
> Siamese neural network is a **class of neural network architectures that contain two or more identical subnetworks**. identical here means they have the same configuration with the same parameters and weights. Parameter updating is mirrored across both subnetworks. Siamese NNs are popular among **tasks that involve finding similarity or a relationship between two comparable things**.

[Degree matrix or diagonal degree matrix](https://en.wikipedia.org/wiki/Degree_matrix)
>  The degree matrix is a diagonal matrix which contains information about the degree of each vertex—that is, the number of edges attached to each vertex.

[Adjacency matrix](https://en.wikipedia.org/wiki/Adjacency_matrix)
>  An adjacency matrix is a square matrix used to represent a finite graph. The elements of the matrix indicate whether pairs of vertices are adjacent or not in the graph.

[Laplacian matrix / Symmetric normalized Laplacian](https://en.wikipedia.org/wiki/Laplacian_matrix#Symmetric_normalized_Laplacian)  
> ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/f9007674eecb50de92fe6aadceee5df23c834b66)  
![](https://wikimedia.org/api/rest_v1/media/math/render/svg/4ab36f74a92195f5be3814f444442270977b1f11)

[Chebyshev polynomials of the first kind](https://en.wikipedia.org/wiki/Chebyshev_polynomials#Definition)
> ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/126bc21a36f58717c757e943d05a04d0091feeb2)

### Methodology:
[Check this blog](https://ht93.github.io/2017/08/13/Graph-Convolution-Basic/)

### Dataset & preprocess:
* **Dataset**: Autism Brain Imaging Data Exchange (ABIDE)  
* **Preprocess pipeling**: Configurable Pipeline for the Analysis of Connectomes (C-PAC)  
```
Including:
* skull striping
* slice timing correction
* motion correction
* global mean intensity normalisation 
* nuisance signal regression 
* band-pass filtering (0.01-0.1Hz)
* registration of fMRI images to standard anatomical space (MNI152)
```
    * After this, they extract the mean time series for a set of regions from the Harvard Oxford (HO) atlas comprising R = 110 cortical and subcortical ROIs and normalise them to zero mean and unit variance.  
* **Number**:
```
Subjects number: N = 871 
ASD disease: 403 
Healthy controls: 468 
Sites number: 20
(from different imaging sites, 871 met the imaging quality and phenotypic information criteria)

```

### Network detail:
* **Task**: measure the similarity between two graph
* **Graph**:
    * **Vertex**: Each ROI is represent by a node $$\mathcal{v}_i\in\mathcal{V}$$
    * **Input feature**: for each ROI, the input feature is the corresponding row of correlation matrix for that ROI.
    * **Edge & weight**: 
        * In **their paper**, they claim that they use $$e_{ij}=d(v_i,v_j)=\sqrt{||v_i-v_j||^2}$$ for weight
        * In **their code**, they used $$W_{ij} = \begin{cases} \exp(-\frac{[dist(i,j)]^2}{2\theta^2}), & \text{if $dist(i,j)\le\mathcal{k}$} \ 0, & \text{otherwise} \end{cases}$$ for weight
        * The edge is determined by k-NN (k-nearest neighbors).
* **Network Structure**:
    1. **CNN**:
        1. 2 layers with 64 features (shared in Siamese network)
        2. K=3, convolution takes input at most K steps away from a node.
    2. **FC**:
        1. One output with Sigmoid activation $${\displaystyle S(x)={\frac {1}{1+e^{-x}}}={\frac {e^{x}}{e^{x}+1}}.}$$
        2. A binary feature is introduced at the FC layer indicating whether the subject pair were scanned at the same site or not.
        3. Dropout 0.2 on FC
* **Loss function**:

$$$$

* **Network detail**:
    * **Adam optimizer**: 0.001 learning rate and 0.005 regularization
    * **Loss function**: margin m=0.6, weight lambda=0.35
    * **mini-batch**: 200
* **Train and test**: 
    1. 871 total, 720 train, 151 test.
    2. train form 21802 matching and 21398 non-matching graph pairs. test form  5631 matching and 5694 non-matching.
    3. all graphs are fed to the network the same number of times to avoid biases.
    4. subjects from all 20 sites are included in both training and test sets

### Results:
In order to demonstrate the learned metric's ability to facilitate a subject classification task (ASD vs control), we use a simple
k-nn classifier with k = 3 based the estimated distances. Improvement in classification scores reaches 11.9% on the total test set and up to 40% for individual sites (Compared with PCA/Euclidean distance).

### Unsolved Question:
* An additional l2 regularisation term on the weights of the fully connected layer is introduced to the loss function.
> [some answer](https://github.com/fchollet/keras/issues/5673) Your question indicates you are confused about how regularization works. L2 regularization is essentially a zero mean isotropic Gaussian prior on weights so it is applied to the weights in the layers you specify. The regularization takes the form of a penalty that is added to the loss function, so they are jointly minimized.

### Personal thought:
* What is the point of Siamese network? Why not just a classifier with GCN and FC (or maybe the performance is not good in this way?)?
