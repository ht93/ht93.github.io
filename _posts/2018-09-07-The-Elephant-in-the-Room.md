---
layout: post
title: The Elephant in the Room
---

Paper: [arxiv](https://arxiv.org/abs/1808.03305)  
Code: 
Published: 9 Aug 2018

### Key Idea:
> We showcase a family of common failures of state-of-the art object detectors, “object transplanting”.

### Backgroung knowledge:
Object transplanting
> putting (“transplanting”)
an object from one image in a new location of another image

### Experiment 1:
* Using state-of-the-art object detection method (Faster-RCNN [9] with a NASNet backbone [20])
* Image of a living-room from the Microsoft COCO object detection benchmark
* Transplant "elephant" (refer as T) into this image at various locations
* several interesting phenomena
  1. Detection is not stable: the object may occasionaly become undetected or be detected with sharp changes in confidence
  2. The reported identity of the object T is not consistent (chair in 1,f): the object may be detected as a variety of different classes depending on location
  3. The object causes non-local effects: objects nonoverlapping with T can switch identity, boundingbox, or disappear altogether.

### Detailed Analysis of these 3 findings:
* Dataset: 2017 version of the MS-COCO dataset
* Models: models from the Tensorflow Object Detection API (Table II)
* Test Image Generation: picking a random pair of images I, J and transplanting a random object from the image J into image I
  * Randomly pick image J $$\ne$$ I
  * Randomly select a object in J "transplant" to a series of location $$t_x, t_y$$ in image I
  * Dectect result: $$D_{x,y}$$ with bounding box coordinates, detection score and object category
  * Confidently detected object classes: $$C_{x,y}$$
* Matching Detections: $$\vert C_{x,y}\setminus C_{\null}\vert$$


### Dataset & preprocess:
* **Dataset**: Autism Brain Imaging Data Exchange (ABIDE) & UK Biobank (UKB)
* **Preprocess pipeling**: 
  * ABIDE: Configurable Pipeline for the Analysis of Connectomes (C-PAC)

<pre>
  <code class="markdown">
  Including:
    * skull striping
    * slice timing correction
    * motion correction
    * global mean intensity normalisation 
    * nuisance signal regression 
    * band-pass filtering (0.01-0.1Hz)
    * registration of fMRI images to standard anatomical space (MNI152)
  </code>
</pre>
  
  * UKB: Miller2016

* **ROI**: 
  * ABIDE: Harvard Oxford (HO) atlas (R = 110 cortical and subcortical ROIs)
    * Extract the mean time series for ROI
    * Normalised to zero mean and unit variance. 
  * UKB: 55 (100 spatially independent components, 55 non artefactual. Miller2016)
* **Number**:
  * ABIDE:

<pre>
  <code class="markdown">
  Subjects number: N = 871 
  ASD disease: 403 
  Healthy controls: 468 
  Sites number: 20
  (from different imaging sites, 871 met the imaging quality and phenotypic information criteria)
  </code>
</pre>
  
  * UKB:
  
<pre>
  <code class="markdown">
  Subjects number: N = 2500
  Male: 1181 
  Female: 1319 
  </code>
</pre>

### Network detail:
* **Task**: measure the similarity between two graph
* **Graph**:
    * **Vertex**: Each ROI is represent by a node $$\mathcal{v}_i\in\mathcal{V}$$
    * **Input feature**: for each ROI, the input feature is the corresponding row of correlation matrix for that ROI.
    * **Edge & weight**: 
        * Type 1: Spatial distance as graph $$e_{ij}=d(v_i,v_j)=\sqrt{\|v_i-v_j\|^2}$$ for weight
        * Type 2: mean functional connectivity as graph
        * The edge is determined by k-NN (k-nearest neighbors). k=10
* **Network Structure**:
    1. **CNN**:
        1. 2 layers with 64 features (shared in Siamese network)
        2. K=3, convolution takes input at most K steps away from a node.
    2. **FC**:
        1. One output with Sigmoid activation $${\displaystyle S(x)={\frac {1}{1+e^{-x}}}={\frac {e^{x}}{e^{x}+1}}.}$$
        2. A binary feature is introduced at the FC layer indicating whether the subject pair were scanned at the same site or not.
        3. Dropout 0.2/0.5 (ABIDE/UKB) on FC
* **Loss function**:
  * global loss:  
  $$J^g=(\sigma^{2+}+\sigma^{2-})+\lambda max(0,m-(\mu^+-\mu^-))$$

  It maximises the mean similarity $$\mu^+$$ between embeddings belonging to the same class, minimises the mean similarity between embeddings belonging to different classes $$\mu^-$$. And minimises the variance of pairwise similarities for both matching $$\sigma^{2+}$$ and non-matching $$\sigma^{2-}$$ pairs of graphs.  
  * constrained variance loss:  
  $$J^g=max(0,\sigma^{2+}-a)+max(0,\sigma^{2-}-a)+\lambda max(0,m-(\mu^+-\mu^-))$$
  Compare to global loss, it add a threshold a to the variance.
  
* **Network detail**:
    * **Adam optimizer**: 0.001 learning rate and 0.0005/0.05 (ABIDE/UKB) regularization
    * **Loss function**: margin m=1.0, weight lambda=1.0, a=m/2
    * **mini-batch**: 200
* **Train and test**: 
  * ABIDE:
    1. 871 total, 720 train, 151 test.
    2. train form 21802 matching and 21398 non-matching graph pairs. test form  5631 matching and 5694 non-matching.
    3. all graphs are fed to the network the same number of times to avoid biases.
    4. subjects from all 20 sites are included in both training and test sets
  * UKB:
    1. 5 fold cross validation
    2. 2500 total, 2000 train, 500 test
