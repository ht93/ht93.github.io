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
> $$L^2([-1,1],dy/\sqrt(1-y^2))$$

2. [Square-integrable function](https://en.wikipedia.org/wiki/Square-integrable_function)
> In mathematics, a square-integrable function, also called a quadratically integrable function, is a real- or complex-valued measurable function for which the integral of the square of the absolute value is finite.
> A space which is complete under the metric induced by a norm is a Banach space. Therefore, the space of square integrable functions is a Banach space, under the metric induced by the norm, which in turn is induced by the inner product. As we have the additional property of the inner product, this is specifically a Hilbert space, because the space is complete under the metric induced by the inner product.
