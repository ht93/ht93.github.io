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
