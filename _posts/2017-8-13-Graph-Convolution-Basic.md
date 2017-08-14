---
layout: post
title: Graph Convolution Basic
---

# Graph

$$\begin{align}
\text{Graph} \ &G=\{\mathcal{V}, \mathcal{E}, \mathbf{W}\} \\
&\mathcal{V} \text{, set of vertices}\\
&\mathcal{E} \text{, set of edges}\\
&\mathbf{W} \text{, weighted adjacency matrix}
\end{align}$$  

Edge $$e=(i,j)$$ connects vertices $$v_i$$ and $$v_j$$. It has weight of $$\mathbf{W}_{ij}$$

$$\begin{align}
&N \text{, number of vertices, }|\mathcal{V}|=N\\
&\mathbf{D} \text{, diagonal degree matrix}, D_{ii}=\sum_j\mathbf{W}_{ij}\\
&\L=\mathbf{D}-\mathbf{W} \text{, Non normalized graph laplacian}\\
&(\L f)(i) = \sum_{j\in N_i} \mathbf{W_{ij}}[f(i)-f(j)]\\
&\tilde\L=\mathbf{D}^{-1/2}\L\mathbf{D}^{-1/2}=\mathbf{I_N}-\mathbf{D}^{-1/2}\mathbf{W}\mathbf{D}^{-1/2} \text{, Normalized graph laplacian}\\
&(\tilde\L f)(i) = \frac{1}{\sqrt{d_i}}\sum_{j\in N_i} \mathbf{W_{ij}}\bigg[\frac{f(i)}{\sqrt{d_i}}-\frac{f(i)}{\sqrt{d_j}}\bigg]
\end{align}$$  

Laplacian is a real symmetric matrix.  
It has orthonormal eigenvector as $$\{\mathbf{u}_l\}_{l=0,1,...,N-1}$$ and eigenvalue $$\{\lambda_l\}_{l=0,1,...,N-1}$$.  

$$\begin{align}
\L \mathbf{u}_l &= \lambda \mathbf{u}_l\\
\L &= \mathbf{U}\mathbf{\Lambda}\mathbf{U}^T \text{, where } \mathbf{\Lambda}=diag(\lambda_0,...,\lambda_{N-1}), 
\mathbf{U}= 
\begin{bmatrix}
        |            & \cdots & | \\[0.3em]
        \mathbf{u}_0 & \cdots & \mathbf{u}_{N-1} \\[0.3em]
        |            & \cdots & |
\end{bmatrix}
\end{align}$$  
Assume $$0=\lambda_0<\lambda_1\le\lambda_2...\le\lambda_{N-1}:=\lambda_{max}$$, we denote the entire spectrum by $$\sigma(\L):=\{\lambda_0,\lambda_1,...,\lambda_{N-1}\}$$  
