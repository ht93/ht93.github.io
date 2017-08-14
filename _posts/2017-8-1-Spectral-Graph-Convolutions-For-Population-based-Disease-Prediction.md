---
layout: post
title: Spectral Graph Convolutions for Population based Disease Prediction
---

Paper: [arxiv](https://arxiv.org/abs/1703.03020)  
Code: [github](https://github.com/parisots/population-gcn) (tensorflow)

### Key idea:
Graph Convolutional Networks (GCN) for brain analysis in populations, combining imaging and non-imaging data.

### Network Outline:
**objective**: to assign to each acquisition, corresponding to a subject and time point, a label l ∈ L describing the corresponding subject’s disease state (e.g. control or diseased).  
**vertex**: We represent the population as a graph where each subject is associated with an imaging feature vector and corresponds to a graph vertex.   
**edge**: The graph edge weights are derived from phenotypic data, and encode the pairwise similarity between subjects and the local neighbourhood system.  
Considering a set of H non-imaging measures $$M={M_h}$ (e.g. subject’s gender and age), the population graph’s adjacency matrix W is defined as follows:  
$$W(v,w)=Sim(S_v,S_w)\sum_{h=1}^H \rho(M_h(v),M_h(w))$$
 
**GCN**: check this paper [Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering](https://ht93.github.io/2017/07/30/Convolutional-Neural-Networks-On-Graphs-With-Fast-Localized-Spectral-Filtering/)  
**Training**: This structure is used to train a GCN model on partially labelled graphs, aiming to infer the classes of unlabelled nodes from the node features and pairwise associations between subjects.

### Detail & Result:
#### Autism Brain Imaging Data Exchange (ABIDE)
* **Task**: classify subjects healthy or suffering from Autism Spectrum Disorders (ASD).
* **Objective**: exploit the acquisition information which can strongly affect the comparability of subjects.
* **Dataset**: [ABIDE](http://fcon_1000.projects.nitrc.org/indi/abide/)
* **Dataset Detail**:
  1. 871 subjects, 403 ASD and 468 healthy controls.
  2. 20 different sites
  3. Preprocessing pipeline from C-PAC & ROI from Harvard Oxford (HO) atlas, same as [Ruckert2016](https://ht93.github.io/2017/07/24/Distance-Metric-Learning-using-Graph-Convolutional-Networks-Application-to-Functional-Brain-Networks/#dataset--preprocess)
  4. The individual connectivity matrices are estimated by computing the Fisher transformed Pearson’s correlation coefficient between the representative rs-fMRI timeseries of each ROI in the HO atlas.
* **Input feature**: vectorised functional connectivity matrix. And a [ridge classifier](http://scikit-learn.org/stable/modules/linear_model.html#ridge-regression) is employed to select the most discriminative features from the training set.
* **Graph**: 
* **Result**: We show how integrating acquisition information allows to outperform the current state of the art on the whole dataset with a global accuracy of 69.5%.  
#### Alzheimer's Disease Neuroimaging Initiative (ADNI)
* **Task**: predict whether an MCI patient will convert to AD. 
* **Objective**: demonstrate the importance of exploiting longitudinal information, which can be easily integrated into our graph structure, to increase performance.
* **Dataset**: [ADNI](http://adni.loni.usc.edu/)
* **Dataset Detail**:
  1. 540 subjects (1675 samples) with early/late MCI and contained longitudinal T1 MR images, 289 subjects (843 samples) diagnosed as AD
  2. Acquisitions after conversion to AD were not included.
* **Input feature**: volumes of all 138 segmented brain structures
* **Result**: seamlessly integrate longitudinal data and provides a significant increase in performance to 77% accuracy for the challenging task of predicting the conver- sion from Mild Cognitive Impairment (MCI) to Alzheimer’s Disease (AD).
