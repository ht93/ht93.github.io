---
layout: post
title: Graph Convolution Basic
---

\\documentclass{article}\
\\usepackage{geometry,amssymb,amsmath}\
\\usepackage\[utf8\]{inputenc}\
\\geometry{a4paper,portrait,left=1.0cm,right=1.0cm,top=2.0cm,bottom=2.0cm}

\\begin{document}

\\section{Graph}

\\begin{align}\
\\text{Graph} \\ &G={\\mathcal{V}, \\mathcal{E}, \\mathbf{W}} \\\
&\\mathcal{V} \\text{, set of vertices}\\\
&\\mathcal{E} \\text{, set of edges}\\\
&\\mathbf{W} \\text{, weighted adjacency matrix}\
\\end{align}\
Edge \$e=(i,j)\$ connects vertices \$v\_i\$ and \$v\_j\$. It has weight
of \$\\mathbf{W}\_{ij}\$

\\begin{align}\
&N \\text{, number of vertices, }|\\mathcal{V}|=N\\\
&\\mathbf{D} \\text{, diagonal degree matrix},
D\_{ii}=\\sum\_j\\mathbf{W}*{ij}\\\
&\\L=\\mathbf{D}-\\mathbf{W} \\text{, Non normalized graph laplacian}\\\
&(\\L f)(i) = \\sum*{j\\in N\_i} \\mathbf{W\_{ij}}\[f(i)-f(j)\]\\\
&\\tilde\\L=\\mathbf{D}\^{-1/2}\\L\\mathbf{D}\^{-1/2}=\\mathbf{I\_N}-\\mathbf{D}\^{-1/2}\\mathbf{W}\\mathbf{D}\^{-1/2}
\\text{, Normalized graph laplacian}\\\
&(\\tilde\\L f)(i) = \\frac{1}{\\sqrt{d\_i}}\\sum\_{j\\in N\_i}
\\mathbf{W\_{ij}}\\bigg\[\\frac{f(i)}{\\sqrt{d\_i}}-\\frac{f(i)}{\\sqrt{d\_j}}\\bigg\]\
\\end{align}\
Laplacian is a real symmetric matrix. \\\
It has orthonormal eigenvector as \${\\mathbf{u}*l}*{l=0,1,...,N-1}\$
and eigenvalue \${\\lambda\_l}\_{l=0,1,...,N-1}\$.

\\begin{align}\
\\L \\mathbf{u}*l &= \\lambda \\mathbf{u}*l\\\
\\L &= \\mathbf{U}\\mathbf{\\Lambda}\\mathbf{U}\^T \\text{, where }
\\mathbf{\\Lambda}=diag(\\lambda\_0,...,\\lambda*{N-1}),\
\\mathbf{U}=\
\\begin{bmatrix}\
| & \\cdots & | \\\[0.3em\]\
\\mathbf{u}*0 & \\cdots & \\mathbf{u}*{N-1} \\\[0.3em\]\
| & \\cdots & |\
\\end{bmatrix}\
\\end{align}\
Assume
\$0=\\lambda\_0&lt;\\lambda\_1\\le\\lambda\_2...\\le\\lambda*{N-1}:=\\lambda\_{max}\$,
we denote the entire spectrum by
\$\\sigma(\\L):={\\lambda\_0,\\lambda\_1,...,\\lambda\_{N-1}}\$

\\section{Graph Fourier transform}\
Eigenfunction of a linear operator \$D\$ is any non-zero function \$f\$
that \$Df=\\lambda f\$, where \$\\lambda\$ is a scaling factor called
eigenvalue.\\\\\
In one dimensional space, laplacian or laplace operator is \$\\Delta\$:\
\\begin{align}\
\\Delta f=\\nabla\^2f=\\nabla \\nabla f \\text{ where }
\\nabla=(\\frac{\\partial}{\\partial
x\_1},...,\\frac{\\partial}{\\partial x\_n})\
\\end{align}\
For classical Fourier transform:\
\\begin{align}\
\\hat f(\\xi)=<f,e^{2\pi i\xi t}>=\\int\_\\mathbb{R}f(t)e\^{-2\\pi i\\xi
t}dt\
\\end{align}\
where \$e\^{2\\pi i\\xi t}\$ is eigenfunction of \$\\Delta\$, since,\
\\begin{align}\
-\\Delta (e\^{2\\pi i\\xi t})=-\\frac{\\partial\^2}{\\partial
t\^2}e\^{2\\pi i\\xi t}=(2\\pi \\xi)\^2 e\^{2\\pi i\\xi t}\
\\end{align}\
For graph, we can define \\textbf{graph Fourier transform} \$\\hat f\$
as,\
\\begin{align}\
\\hat f(\\lambda\_l):=&lt;\\mathbf{f},\\mathbf{u}*l&gt;=\\sum*{i=1}\^N
f(i) u\^\**l(i)\
\\end{align}\
and \\textbf{inverse graph Fourier transform} as,\
\\begin{align}\
f(i)=\\sum*{l=0}\^{N-1} \\hat f(\\lambda\_l) u\_l(i)\
\\end{align}

\\section{Graph Spectral Filtering}\
In classical signal processing, filter is:\
\\begin{align}\
\\hat f\_{out} (\\xi)=\\hat f\_{in} (\\xi) \\hat h(\\xi)\
\\end{align}\
where \$\\hat h(\\cdot)\$ is transfer function of this filter. By
inverse Fourier transform,\
\\begin{align}\
f\_{out}(t)&=\\int\_{\\mathbb R} \\hat f\_{in}(\\xi) \\hat h(\\xi)
e\^{2\\pi i\\xi t}d\\xi\\\
&= \\int\_{\\mathbb R} f\_{in}(\\tau)h(t-\\tau)d\\tau =:
(f\_{in}\*h)(t)\
\\end{align}\
For graph, we define \\textbf{graph spectral} (frequency)
\\textbf{filtering} as,\
\\begin{align}\
\\hat f\_{out} (\\lambda\_l)=\\hat f\_{in} (\\lambda\_l) \\hat
h(\\lambda\_l)\
\\end{align}\
by inverse graph Fourier transform,\
\\begin{align}\
f\_{out}(i)=\\sum\_{l=0}\^{N-1} \\hat f\_{in} (\\lambda\_l) \\hat
h(\\lambda\_l) u\_l(i)\
\\end{align}\
With some matrix manipulation and orthonormality, we can get:\
\\begin{align}\
\\mathbf{f}*{out}=\\hat h(\\L)\\mathbf{f}*{in} \\text{, where } \\hat
h(\\L):=\\mathbf{U}\
\\begin{bmatrix}\
\\hat h(\\lambda\_0) & & \\mathbf{0} \\\[0.3em\]\
& \\ddots & \\\[0.3em\]\
\\mathbf{0} & & \\hat h(\\lambda\_{N-1})\
\\end{bmatrix}\
\\mathbf{U}\^T\
\\end{align}\
Based on equation (20), we also define (22) or (23) as convolution on
graph.

\\section{Chebyshev polynomial expansion}\
Note: we are using normalized graph laplacian now.\\\\\
Assume we have a filter \$g\_\\theta = diag(\\theta)\$, parameterized by
\$\\theta \\in \\mathbb R\^N\$\
\\begin{align}\
y=g\_\\theta*x=\\mathbf{U}g\_\\theta\\mathbf{U}\^Tx\
\\end{align}\
In order to calculate this, we need calculate
\$\\mathbf{U}g\_\\theta\\mathbf{U}\^T. \~ (O(N\^2))\$ \\\\\
To avoid this, we can treat \$g\_\\theta\$ as a function of
\$\\mathbf{\\Lambda}\$, and approximate it by truncated expansion in
terms of Chebyshev polynomial \$T\_k(x)\$ up to \$K\^{th}\$ order:\
\\begin{align}\
g\_{\\theta'}(\\mathbf\\Lambda)\\approx\\sum\_{k=0}\^K \\theta'*k T\_k
(\\tilde{\\mathbf\\Lambda})\
\\end{align}\
with rescaled \$\\tilde{\\mathbf\\Lambda} =
\\frac{2}{\\mathbf{\\lambda}*{max}}\\mathbf{\\Lambda}-I\_N\$,
\$\\theta'*k\$ as vector of Chebyshev coefficients, and:\
\\begin{align}\
T*{0}(x)&=1\\T\_{1}(x)&=x\\T\_{n+1}(x)&=2xT\_{n}(x)-T\_{n-1}(x).\
\\end{align}\
\\begin{align}\
\\mathbf{U}g\_{\\theta'}(\\mathbf\\Lambda)\\mathbf{U}\^T \\approx
\\sum\_{k=0}\^K \\theta'*k \\mathbf{U} T\_k (\\tilde{\\mathbf\\Lambda})
\\mathbf{U}\^T = \\sum*{k=0}\^K \\theta'*k T\_k (\\tilde{\\L}) \\text{,
where } \\tilde{\\L} = \\frac{2}{\\mathbf{\\lambda}*{max}}\\L-I\_N\
\\end{align}\
since
\$(\\mathbf{U}\\mathbf{\\Lambda}\\mathbf{U}\^T)\^k=\\mathbf{U}\\mathbf{\\Lambda}\^k\\mathbf{U}\^T\$\\\
Here we have:\
\\begin{align}\
g\_{\\theta'}*x \\approx \\sum\_{k=0}\^K \\theta'\_k T\_k (\\tilde{\\L})
x\
\\end{align}\
Noted this expression is K-localized since it is a \$K\^{th}\$ order
polynomial of laplacian.\
\\end{document}
