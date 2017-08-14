---
layout: post
title: Graph Convolution Basic
---

# Graph

\[\begin{aligned}
\text{Graph} \ &G=\{\mathcal{V}, \mathcal{E}, \mathbf{W}\} \\
&\mathcal{V} \text{, set of vertices}\\
&\mathcal{E} \text{, set of edges}\\
&\mathbf{W} \text{, weighted adjacency matrix}\end{aligned}\]

Edge \(e=(i,j)\) connects vertices \(v_i\) and \(v_j\). It has weight of
\(\mathbf{W}_{ij}\)

\[\begin{aligned}
&N \text{, number of vertices, }|\mathcal{V}|=N\\
&\mathbf{D} \text{, diagonal degree matrix}, D_{ii}=\sum_j\mathbf{W}_{ij}\\
&\L=\mathbf{D}-\mathbf{W} \text{, Non normalized graph laplacian}\\
&(\L f)(i) = \sum_{j\in N_i} \mathbf{W_{ij}}[f(i)-f(j)]\\
&\tilde\L=\mathbf{D}^{-1/2}\L\mathbf{D}^{-1/2}=\mathbf{I_N}-\mathbf{D}^{-1/2}\mathbf{W}\mathbf{D}^{-1/2} \text{, Normalized graph laplacian}\\
&(\tilde\L f)(i) = \frac{1}{\sqrt{d_i}}\sum_{j\in N_i} \mathbf{W_{ij}}\bigg[\frac{f(i)}{\sqrt{d_i}}-\frac{f(i)}{\sqrt{d_j}}\bigg]\end{aligned}\]

Laplacian is a real symmetric matrix.  
It has orthonormal eigenvector as \(\{\mathbf{u}_l\}_{l=0,1,...,N-1}\)
and eigenvalue \(\{\lambda_l\}_{l=0,1,...,N-1}\).

\[\begin{aligned}
\L \mathbf{u}_l &= \lambda \mathbf{u}_l\\
\L &= \mathbf{U}\mathbf{\Lambda}\mathbf{U}^T \text{, where } \mathbf{\Lambda}=diag(\lambda_0,...,\lambda_{N-1}), 
\mathbf{U}= 
\begin{bmatrix}
        |            & \cdots & | \\[0.3em]
        \mathbf{u}_0 & \cdots & \mathbf{u}_{N-1} \\[0.3em]
        |            & \cdots & |
\end{bmatrix}\end{aligned}\]

Assume
\(0=\lambda_0<\lambda_1\le\lambda_2...\le\lambda_{N-1}:=\lambda_{max}\),
we denote the entire spectrum by
\(\sigma(\L):=\{\lambda_0,\lambda_1,...,\lambda_{N-1}\}\)

# Graph Fourier transform

Eigenfunction of a linear operator \(D\) is any non-zero function \(f\)
that \(Df=\lambda f\), where \(\lambda\) is a scaling factor called
eigenvalue.  
  
In one dimensional space, laplacian or laplace operator is \(\Delta\):
\[\begin{aligned}
\Delta f=\nabla^2f=\nabla \nabla f \text{ where } \nabla=(\frac{\partial}{\partial x_1},...,\frac{\partial}{\partial x_n})\end{aligned}\]
For classical Fourier transform: \[\begin{aligned}
\hat f(\xi)=<f,e^{2\pi i\xi t}>=\int_\mathbb{R}f(t)e^{-2\pi i\xi t}dt\end{aligned}\]
where \(e^{2\pi i\xi t}\) is eigenfunction of \(\Delta\), since,
\[\begin{aligned}
-\Delta (e^{2\pi i\xi t})=-\frac{\partial^2}{\partial t^2}e^{2\pi i\xi t}=(2\pi \xi)^2 e^{2\pi i\xi t} \end{aligned}\]
For graph, we can define **graph Fourier transform** \(\hat f\) as,
\[\begin{aligned}
\hat f(\lambda_l):=<\mathbf{f},\mathbf{u}_l>=\sum_{i=1}^N f(i) u^*_l(i) \end{aligned}\]
and **inverse graph Fourier transform** as, \[\begin{aligned}
f(i)=\sum_{l=0}^{N-1} \hat f(\lambda_l) u_l(i) \end{aligned}\]

# Graph Spectral Filtering

In classical signal processing, filter is: \[\begin{aligned}
\hat f_{out} (\xi)=\hat f_{in} (\xi) \hat h(\xi) \end{aligned}\] where
\(\hat h(\cdot)\) is transfer function of this filter. By inverse
Fourier transform, \[\begin{aligned}
f_{out}(t)&=\int_{\mathbb R} \hat f_{in}(\xi) \hat h(\xi) e^{2\pi i\xi t}d\xi\\
&= \int_{\mathbb R} f_{in}(\tau)h(t-\tau)d\tau =: (f_{in}*h)(t)\end{aligned}\]
For graph, we define **graph spectral** (frequency) **filtering** as,
\[\begin{aligned}
\hat f_{out} (\lambda_l)=\hat f_{in} (\lambda_l) \hat h(\lambda_l) \end{aligned}\]
by inverse graph Fourier transform, \[\begin{aligned}
f_{out}(i)=\sum_{l=0}^{N-1} \hat f_{in} (\lambda_l) \hat h(\lambda_l) u_l(i)\end{aligned}\]
With some matrix manipulation and orthonormality, we can get:
\[\begin{aligned}
\mathbf{f}_{out}=\hat h(\L)\mathbf{f}_{in} \text{, where } \hat h(\L):=\mathbf{U}
\begin{bmatrix}
    \hat h(\lambda_0) &        & \mathbf{0} \\[0.3em]
                      & \ddots &  \\[0.3em]
    \mathbf{0}        &        & \hat h(\lambda_{N-1})
\end{bmatrix}
\mathbf{U}^T\end{aligned}\] Based on equation (20), we also define (22)
or (23) as convolution on graph.

# Chebyshev polynomial expansion

Note: we are using normalized graph laplacian now.  
  
Assume we have a filter \(g_\theta = diag(\theta)\), parameterized by
\(\theta \in \mathbb R^N\) \[\begin{aligned}
y=g_\theta*x=\mathbf{U}g_\theta\mathbf{U}^Tx\end{aligned}\] In order to
calculate this, we need calculate
\(\mathbf{U}g_\theta\mathbf{U}^T. ~ (O(N^2))\)  
  
To avoid this, we can treat \(g_\theta\) as a function of
\(\mathbf{\Lambda}\), and approximate it by truncated expansion in terms
of Chebyshev polynomial \(T_k(x)\) up to \(K^{th}\) order:
\[\begin{aligned}
g_{\theta'}(\mathbf\Lambda)\approx\sum_{k=0}^K \theta'_k T_k (\tilde{\mathbf\Lambda})\end{aligned}\]
with rescaled
\(\tilde{\mathbf\Lambda} = \frac{2}{\mathbf{\lambda}_{max}}\mathbf{\Lambda}-I_N\),
\(\theta'_k\) as vector of Chebyshev coefficients, and:
\[\begin{aligned}
T_{0}(x)&=1\\T_{1}(x)&=x\\T_{n+1}(x)&=2xT_{n}(x)-T_{n-1}(x).\end{aligned}\]
\[\begin{aligned}
\mathbf{U}g_{\theta'}(\mathbf\Lambda)\mathbf{U}^T \approx \sum_{k=0}^K \theta'_k \mathbf{U} T_k (\tilde{\mathbf\Lambda}) \mathbf{U}^T = \sum_{k=0}^K \theta'_k T_k (\tilde{\L}) \text{, where } \tilde{\L} = \frac{2}{\mathbf{\lambda}_{max}}\L-I_N\end{aligned}\]
since
\((\mathbf{U}\mathbf{\Lambda}\mathbf{U}^T)^k=\mathbf{U}\mathbf{\Lambda}^k\mathbf{U}^T\)  
Here we have: \[\begin{aligned}
g_{\theta'}*x \approx \sum_{k=0}^K \theta'_k T_k (\tilde{\L}) x\end{aligned}\]
Noted this expression is K-localized since it is a \(K^{th}\) order
polynomial of laplacian.
