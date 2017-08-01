Paper: [arxiv](https://arxiv.org/abs/1703.03020)  
Code: [github](https://github.com/parisots/population-gcn) (tensorflow)

### Key idea:
Graph Convolutional Networks (GCN) for brain analysis in populations, combining imaging and non-imaging data.

### Detail:
**vertex**: We represent the population as a graph where each subject is associated with an imaging feature vector and corresponds to a graph vertex.   
**edge**: The graph edge weights are derived from phenotypic data, and encode the pairwise similarity between subjects and the local neighbourhood system.  
**GCN**: check this paper [Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering](https://ht93.github.io/2017/07/30/Convolutional-Neural-Networks-On-Graphs-With-Fast-Localized-Spectral-Filtering/)
**Training**: This structure is used to train a GCN model on partially labelled graphs, aiming to infer the classes of unlabelled nodes from the node features and pairwise asso- ciations between subjects.

### Datasets & Result:
**Autism Brain Imaging Data Exchange (ABIDE)**: classify subjects healthy or suffering from Autism Spectrum Disorders (ASD). The ABIDE dataset comprises highly heterogeneous functional MRI data acquired at multiple sites. We show how integrating acquisition information allows to outperform the current state of the art on the whole dataset with a global accuracy of 69.5%.  
**Alzheimer's Disease Neuroimaging Initiative (ADNI)**: seamlessly integrate longitudinal data and provides a significant increase in performance to 77% accuracy for the challenging task of predicting the conver- sion from Mild Cognitive Impairment (MCI) to Alzheimer’s Disease (AD).
