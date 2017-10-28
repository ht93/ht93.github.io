---
layout: post
title: Convolutional, Long Short-Term Memory, fully connected Deep Neural Networks
---

Paper: [IEEE](http://ieeexplore.ieee.org/document/7178838/)  

### Key idea:
We take advantage of the complementarity of CNNs, LSTMs and DNNs by combining them into one unified architecture.

### Background knowledge:
* [log-mel filterbank feature](http://haythamfayek.com/2016/04/21/speech-processing-for-machine-learning.html)
* [LSTMP - LSTM with Recurrent Projection Layer](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43905.pdf):  
    > By setting n_r < n_c we can increase the model memory (n_c) and still be able to control the number of parameters in the recurrent connections and output layer. n_r is the number of units in the recurrent projection layer

### Input 
For each input at time `t`:
* Input $$[x_{x-l},...,x_{t+r}]$$
    * each $$x_t$$ is a 40-dimensional log-mel filterbank feature.
    * `l` contextual vectors at left
    * `r` contextual vectors at right

### Output
For each output at time `t`:
* Output $$y_{t-5}$$
    * output state label is delayed 5 frames

### Network:
```python
Input = Input(size=(40, r+l+1))
H = Conv2D(256, kernel_size=(9,9))(Input) # filter (9,9) on frequency-time
H = Maxpool2D(pool_size=(3,1), strides=(3,1))(H) # pooling 3 on frequency only
H = Conv2D(256, kernel_size=(4,3))(H)
H_cnn = TimeDistributed(Linear(256))(H) # shared parameters on time
x_t = Input(size=(40, 1))
H = Unknown_add(x_t, H_cnn)
H = LSTMP(832, recurrent_projection_units=512, truncated_bptt_steps=20, return_sequence=True)(H)
H = LSTMP(832, recurrent_projection_units=512, truncated_bptt_steps=20, return_sequence=False)(H)
H = Unknown_add(H, H_cnn)
H = Linear(1024)(H)
H = Linear(1024)(H)
```
* r = 0
* Loss: cross-entropy
* Optimizer: (distributed) asynchronous stochastic gradient descent (ASGD)
* Initialization: Unit variance Gaussian for LSTM
* Learning rates: exponentially decay
* Sequence training in larger data sets.

### Experiments:
* From Table 2, A larger context of 20 hurts performance. Use `l=10`
* From Table 3, unrolling 30 time steps degrades WER.
* From Table 4, it is beneficial to use DNN layers to transform the output of the LSTM layer to a space that is more discriminative and easier to predict output targets.
* From Table 5, CLDNN performs better.
* From Table 6, CLDNN benefits from the better weight initialization (uniform initialization).
* From Table 7, short term feature x_t to the LSTM has better performance. CNN features only to LSTM is sufficient.
* From table 8 & 9, dataset outperform LSTM in larger datasets.

### Pros & Cons:
* Pros:
    * Good result and good performance
    * Good intuition
* Cons:
    * The paper is not very detailed like, it did not fully explain the network (multi-scale addiction and input features). This is troublesome since it did not provide code.
    * The baseline are relatively simple.



