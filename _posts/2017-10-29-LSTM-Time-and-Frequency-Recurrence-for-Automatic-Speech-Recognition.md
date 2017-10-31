---
layout: post
title: LSTM Time and Frequency Recurrence for Automatic Speech Recognition
---

Paper: [Microsoft]https://www.microsoft.com/en-us/research/wp-content/uploads/2016/04/spectral_LSTM.pdf)  

### Key idea:
We propose an extension to LSTMs that performs the recurrence in frequency as well as in time.

### Background knowledge:
* [LSTMP - LSTM with Recurrent Projection Layer](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43905.pdf):  
    > By setting n_r < n_c we can increase the model memory (n_c) and still be able to control the number of parameters in the recurrent connections and output layer. n_r is the number of units in the recurrent projection layer

### Motivation:
* Our model is inspired by the way people read spectrograms.
* In standard systems, the log-filter-bank features are independent of one-another, i.e. switching the positions of two filter-banks won’t affect the performance of the DNN or LSTM.
* However, this is not the case when a human reads a spectrogram: a human relies on both patterns that evolve on time, and frequency, to predict phonemes. Switching the positions of two filter-banks will destroy the frequency-wise patterns.

### Network:
* Input:
    * size: (40, 11)
    * 40: 40-dimensional log-mel filterbank feature
    * 11: 11 frames, 1 center frames and 5 contextual frames at left and right
* Output:
    * 1812 tied-triphone states (senones)
* Structure:
    * For each Input at time t, apply F-LSTM on 40 frequency
    * The output of F-LSTM at time t import to one T-LSTM cell, T-LSTM takes input from all the time t

### F-LTSM:
* For each time step t:
    * Divide total N log-filter-banks at current time into M overlapped chunks and each chunk contains B log filterbanks. C is number of log-filter-banks overlapped between adjacent chunks. $$M=\frac{N-C}{B-C}$$
    * M overlapped chunks as input to M F-LSTM cells. Output as $$h_{0~M-1}$$
    * Merge (concatenate?) $$h_{0~M-1}$$ to a vector h, use this as input to T-LSTM cell at this time t.

### Comparison:
* Against CLDNN:
    * The two approaches both aim to achieve invariance to input distortions, but the pattern detectors in the CNN maintain a constant dimensionality, while the F-LSTM can perform a general frequency warping.
* Against Multidimensional RNN:
    * To summarize, the T-F-LSTM works on multidimensional space separately with simplicity while the multidimensional RNN works jointly on multidimensional space with more powerful modeling.

### Experiments:
* Table 1, F-T-LSTM is better than T-LSTM.
* Table 2, F-LSTM with 24 cells performs best.
* Table 3, stacking multiply time frames to the input of F-LSTM does not improve the performance.
