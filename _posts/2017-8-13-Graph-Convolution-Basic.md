---
layout: post
title: Graph Convolution Basic
---

Graph
=====

$$\\begin{aligned}
\\text{Graph} \\ &G=\\{\\mathcal{V}, \\mathcal{E}, \\mathbf{W}\\} \\\\
&\\mathcal{V} \\text{, set of vertices}\\\\
&\\mathcal{E} \\text{, set of edges}\\\\
&\\mathbf{W} \\text{, weighted adjacency matrix}\\end{aligned}$$

Edge *e* = (*i*, *j*) connects vertices *v*<sub>*i*</sub> and *v*<sub>*j*</sub>. It has weight of **W**<sub>*i**j*</sub>

$$\\begin{aligned}
&N \\text{, number of vertices, }|\\mathcal{V}|=N\\\\
&\\mathbf{D} \\text{, diagonal degree matrix}, D\_{ii}=\\sum\_j\\mathbf{W}\_{ij}\\\\
&\\L=\\mathbf{D}-\\mathbf{W} \\text{, Non normalized graph laplacian}\\\\
&(\\L f)(i) = \\sum\_{j\\in N\_i} \\mathbf{W\_{ij}}\[f(i)-f(j)\]\\\\
&\\tilde\\L=\\mathbf{D}^{-1/2}\\L\\mathbf{D}^{-1/2}=\\mathbf{I\_N}-\\mathbf{D}^{-1/2}\\mathbf{W}\\mathbf{D}^{-1/2} \\text{, Normalized graph laplacian}\\\\
&(\\tilde\\L f)(i) = \\frac{1}{\\sqrt{d\_i}}\\sum\_{j\\in N\_i} \\mathbf{W\_{ij}}\\bigg\[\\frac{f(i)}{\\sqrt{d\_i}}-\\frac{f(i)}{\\sqrt{d\_j}}\\bigg\]\\end{aligned}$$

Laplacian is a real symmetric matrix.
It has orthonormal eigenvector as {**u**<sub>*l*</sub>}<sub>*l* = 0, 1, ..., *N* − 1</sub> and eigenvalue {*λ*<sub>*l*</sub>}<sub>*l* = 0, 1, ..., *N* − 1</sub>.

$$\\begin{aligned}
\\L \\mathbf{u}\_l &= \\lambda \\mathbf{u}\_l\\\\
\\L &= \\mathbf{U}\\mathbf{\\Lambda}\\mathbf{U}^T \\text{, where } \\mathbf{\\Lambda}=diag(\\lambda\_0,...,\\lambda\_{N-1}), 
\\mathbf{U}= 
\\begin{bmatrix}
        |            & \\cdots & | \\\\\[0.3em\]
        \\mathbf{u}\_0 & \\cdots & \\mathbf{u}\_{N-1} \\\\\[0.3em\]
        |            & \\cdots & |
\\end{bmatrix}\\end{aligned}$$

Assume 0 = *λ*<sub>0</sub> &lt; *λ*<sub>1</sub> ≤ *λ*<sub>2</sub>...≤*λ*<sub>*N* − 1</sub> := *λ*<sub>*m**a**x*</sub>, we denote the entire spectrum by $\\sigma(\\L):=\\{\\lambda\_0,\\lambda\_1,...,\\lambda\_{N-1}\\}$

Graph Fourier transform
=======================

Eigenfunction of a linear operator *D* is any non-zero function *f* that *D**f* = *λ**f*, where *λ* is a scaling factor called eigenvalue.
In one dimensional space, laplacian or laplace operator is *Δ*:
$$\\begin{aligned}
\\Delta f=\\nabla^2f=\\nabla \\nabla f \\text{ where } \\nabla=(\\frac{\\partial}{\\partial x\_1},...,\\frac{\\partial}{\\partial x\_n})\\end{aligned}$$
 For classical Fourier transform:
$$\\begin{aligned}
\\hat f(\\xi)=&lt;f,e^{2\\pi i\\xi t}&gt;=\\int\_\\mathbb{R}f(t)e^{-2\\pi i\\xi t}dt\\end{aligned}$$
 where *e*<sup>2*π**i**ξ**t*</sup> is eigenfunction of *Δ*, since,
$$\\begin{aligned}
-\\Delta (e^{2\\pi i\\xi t})=-\\frac{\\partial^2}{\\partial t^2}e^{2\\pi i\\xi t}=(2\\pi \\xi)^2 e^{2\\pi i\\xi t} \\end{aligned}$$
 For graph, we can define **graph Fourier transform** $\\hat f$ as,
$$\\begin{aligned}
\\hat f(\\lambda\_l):=&lt;\\mathbf{f},\\mathbf{u}\_l&gt;=\\sum\_{i=1}^N f(i) u^\*\_l(i) \\end{aligned}$$
 and **inverse graph Fourier transform** as,
$$\\begin{aligned}
f(i)=\\sum\_{l=0}^{N-1} \\hat f(\\lambda\_l) u\_l(i) \\end{aligned}$$

Graph Spectral Filtering
========================

In classical signal processing, filter is:
$$\\begin{aligned}
\\hat f\_{out} (\\xi)=\\hat f\_{in} (\\xi) \\hat h(\\xi) \\end{aligned}$$
 where $\\hat h(\\cdot)$ is transfer function of this filter. By inverse Fourier transform,
$$\\begin{aligned}
f\_{out}(t)&=\\int\_{\\mathbb R} \\hat f\_{in}(\\xi) \\hat h(\\xi) e^{2\\pi i\\xi t}d\\xi\\\\
&= \\int\_{\\mathbb R} f\_{in}(\\tau)h(t-\\tau)d\\tau =: (f\_{in}\*h)(t)\\end{aligned}$$
 For graph, we define **graph spectral** (frequency) **filtering** as,
$$\\begin{aligned}
\\hat f\_{out} (\\lambda\_l)=\\hat f\_{in} (\\lambda\_l) \\hat h(\\lambda\_l) \\end{aligned}$$
 by inverse graph Fourier transform,
$$\\begin{aligned}
f\_{out}(i)=\\sum\_{l=0}^{N-1} \\hat f\_{in} (\\lambda\_l) \\hat h(\\lambda\_l) u\_l(i)\\end{aligned}$$
 With some matrix manipulation and orthonormality, we can get:
$$\\begin{aligned}
\\mathbf{f}\_{out}=\\hat h(\\L)\\mathbf{f}\_{in} \\text{, where } \\hat h(\\L):=\\mathbf{U}
\\begin{bmatrix}
    \\hat h(\\lambda\_0) &        & \\mathbf{0} \\\\\[0.3em\]
                      & \\ddots &  \\\\\[0.3em\]
    \\mathbf{0}        &        & \\hat h(\\lambda\_{N-1})
\\end{bmatrix}
\\mathbf{U}^T\\end{aligned}$$
 Based on equation (20), we also define (22) or (23) as convolution on graph.

Chebyshev polynomial expansion
==============================

Note: we are using normalized graph laplacian now.
Assume we have a filter *g*<sub>*θ*</sub> = *d**i**a**g*(*θ*), parameterized by $\\theta \\in \\mathbb R^N$
$$\\begin{aligned}
y=g\_\\theta\*x=\\mathbf{U}g\_\\theta\\mathbf{U}^Tx\\end{aligned}$$
 In order to calculate this, we need calculate **U***g*<sub>*θ*</sub>**U**<sup>*T*</sup>. (*O*(*N*<sup>2</sup>))
To avoid this, we can treat *g*<sub>*θ*</sub> as a function of **Λ**, and approximate it by truncated expansion in terms of Chebyshev polynomial *T*<sub>*k*</sub>(*x*) up to *K*<sup>*t**h*</sup> order:
$$\\begin{aligned}
g\_{\\theta'}(\\mathbf\\Lambda)\\approx\\sum\_{k=0}^K \\theta'\_k T\_k (\\tilde{\\mathbf\\Lambda})\\end{aligned}$$
 with rescaled $\\tilde{\\mathbf\\Lambda} = \\frac{2}{\\mathbf{\\lambda}\_{max}}\\mathbf{\\Lambda}-I\_N$, *θ*′<sub>*k*</sub> as vector of Chebyshev coefficients, and:
$$\\begin{aligned}
T\_{0}(x)&=1\\\\T\_{1}(x)&=x\\\\T\_{n+1}(x)&=2xT\_{n}(x)-T\_{n-1}(x).\\end{aligned}$$
$$\\begin{aligned}
\\mathbf{U}g\_{\\theta'}(\\mathbf\\Lambda)\\mathbf{U}^T \\approx \\sum\_{k=0}^K \\theta'\_k \\mathbf{U} T\_k (\\tilde{\\mathbf\\Lambda}) \\mathbf{U}^T = \\sum\_{k=0}^K \\theta'\_k T\_k (\\tilde{\\L}) \\text{, where } \\tilde{\\L} = \\frac{2}{\\mathbf{\\lambda}\_{max}}\\L-I\_N\\end{aligned}$$
 since (**U****Λ****U**<sup>*T*</sup>)<sup>*k*</sup> = **U****Λ**<sup>*k*</sup>**U**<sup>*T*</sup>
Here we have:
$$\\begin{aligned}
g\_{\\theta'}\*x \\approx \\sum\_{k=0}^K \\theta'\_k T\_k (\\tilde{\\L}) x\\end{aligned}$$
 Noted this expression is K-localized since it is a *K*<sup>*t**h*</sup> order polynomial of laplacian.
