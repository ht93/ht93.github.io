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
