---
layout: post
title: Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering
---

Paper: [arxiv](https://arxiv.org/abs/1606.09375)
Code: [github](https://github.com/mdeff/cnn_graph)
Submitted: 30 Jun 2016

### Key Idea:
> We present a formulation of CNNs in the context of spectral graph theory, ... design fast localized convolutional filters on graphs.

### Claimed contribution:
1. Spectral formulation: tools from graph signal processing (GSP) on CNN
2. Strictly localized filters: filters strictly localized in a ball of radius K
3. Low computational complexity:  linear complexity w.r.t the input data size n. no Fourier basis, eigenvalue decomposition and store the basis. Only need o store the Laplacian, a sparse matrix of $$\|\Eulerconst\|$$ non-zero values.
4. Efficient pooling: is analog to pooling of 1D signals by rearrangement of the vertices as a binary tree structure.
5. Experimental results

### Background Knowledge:
1. [Hilbert Space](https://en.wikipedia.org/wiki/Hilbert_space#Definition) 
> A Hilbert space H is a real or complex inner product space that is also a complete metric space with respect to the distance function induced by the inner product.  
> [wiki](https://en.wikipedia.org/wiki/Hilbert_space#Lebesgue_spaces) Inner product of functions f and g in $$L^2\bigl([-1,1],dy/\sqrt{(1-y^2)}\bigr)$$ is $$\langle f,g\rangle=\int_{-1}^{1}f(y)\overline{g(y)}\frac{dy}{\sqrt{1-y^2}}$$

2. [Square-integrable function](https://en.wikipedia.org/wiki/Square-integrable_function)
> In mathematics, a square-integrable function, also called a quadratically integrable function, is a real- or complex-valued measurable function for which the integral of the square of the absolute value is finite.  
> A space which is complete under the metric induced by a norm is a Banach space. Therefore, the space of square integrable functions is a Banach space, under the metric induced by the norm, which in turn is induced by the inner product. As we have the additional property of the inner product, this is specifically a Hilbert space, because the space is complete under the metric induced by the inner product.

3. [some detailed Chebyshev calculation](https://arxiv.org/pdf/1105.1891.pdf) check section III part C "The Chebyshev Polynomial Approximation"

### Methodology
##### Fast localized spectral filters


##### Graph coarsening
Use coarsening phase of the Graclus multilevel clustering algorithm with normalized cut as spectral clustering objectives. Graclus’ greedy rule consists, at each coarsening level, in picking an unmarked vertex i and matching it with one of its unmarked neighbors j that maximizes the local normalized cut $$W_{ij}(1/d_i+1/d_j)$$. The two matched vertices are then marked and the coarsened weights are set as the sum of their weights.

##### Fast Pooling of Graph Signals
After coarsening, the nodes in pair would be pooled (in article they use maxpooling), the node not in pair (called singleton) is paired with a fake nodes (initialed with neutral value, e.g. 0, when using ReLU and max pooling). This is pooling of 2, pooling of 4 can be done by 2-pooling 2 times.

### Result and discussion
##### MNIST
Use the image 2D grid for 8-NN (K-nearest neighbourhood) graph, almost same performance as normal CNN on image data (CNN vs GCNN: 99.33% vs 99.14%).  
They say this may due to isotropic nature of the spectral filters, i.e. the fact that edges in a general graph do not possess an orientation (like up, down, right and left for pixels on a 2D grid).
##### 20NEWS
Use bag-of-words and other embedding get good result. 
##### findings
Their Spectral Filter is linear O(n) complexity compared to other filter and easily be paralleled by GPU which get 8 times speedup.
**better Graph Quality, better result**
