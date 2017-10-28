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
    * `l` contextual vectors at left, `l>20`
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
H = TimeDistributed(Linear(256))(H) # shared parameters on time
H = LSTMP(832, recurrent_projection_units=512, truncated_bptt_steps=20, return_sequence=True)(H)
H = LSTMP(832, recurrent_projection_units=512, truncated_bptt_steps=20, return_sequence=False)(H)
H = Linear(1024)(H)
H = Linear(1024)(H)
```
* r = 0
