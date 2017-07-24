Paper: [arxiv](https://arxiv.org/abs/1703.02161)

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
1.


### Question:
An additional l2 regularisation term on the weights of the fully connected layer is introduced to the loss function.
> [some answer](https://github.com/fchollet/keras/issues/5673) Your question indicates you are confused about how regularization works. L2 regularization is essentially a zero mean isotropic Gaussian prior on weights so it is applied to the weights in the layers you specify. The regularization takes the form of a penalty that is added to the loss function, so they are jointly minimized.
