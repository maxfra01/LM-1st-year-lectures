# Introduction to ML

**Pattern** **Recognition**:
>Automatic discovery of regularities in data through the use of computer algorithms (...) to take actions such as classifying the data into different categories

**Machine** **Learning**:
>A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.2

Define models that are able to capture the regularities in our data and allow performing inference about the properties we are interested in. The models should not simply specify a set of human–defined rules, but should be able to learn from data. The learning stage should leverage observed data to improve the quality of the inference.

Applications of ML models are: classification, linear regression, density estimation.

We can identify two branches:
- **Supervised learning**: a model is fed with both input and output data (common in classification and regression): we need to estimate a mapping between input and output data
- **Unsupervised learning**: No feedback is provided to the system (common in clustering, density est.): the goal is to identify some useful structures of data.

Sometimes this methods can be used together: for example we can use unsupervised learning for pre processing steps, then we can use supervised learning.
We will focus on **classification** and **density estimation** (the goal is the estimation of distribution of input data).

# Pattern classification

The goal is to **assign a pattern to a class**: classes represent characteristics of the objects that we are considering 
We can identify two sub-category:
- **binary** classification: common applications are identity verification or intrusion detection
- **multiclass** classification: common applications are speech recognition or objcet categorization
We can also define:
- **closed set classification**: the outcome of the classification will be a possible value in a well-known set ($l_{1},l_{2}\dots l_{L}$)
- **open-set classification**: the outcome can be one value in a well-known set **and also none of them**

The process of **classification** can be divided into steps:

![[Pasted image 20240314171147.png]]

### Feature extraction

The goal of this phase is to **represent** an object in terms of numerical attributes, usually
arranged as vectors or matrices. 
Often it involves complex manipulations to extract a useful representation.

### Dimensionality reduction

The feature space is often very large and contains a large amount of unwanted and potentially harmful information.
Dimensionality reduction techniques compute a mapping from the n–dimensional feature space to a m–dimensional space, with m ≪ n.
Benefits from this phase are:
- Compressed information
- Unwanted data removed (noise)
- It helps data visualization
- It helps classification

In particular, this phase reduces **curse of dimensionality** and **overfitting**:

**Overfitting**:
>An over-complex model fits very accurately the observed data (very small training error), but is not able to provide accurate predictions for unseen data.

**Curse of dimensionality**:
>Volumes in high-dimensional spaces grow very fast, and data becomes very sparse

![[Pasted image 20240316160823.png]]

