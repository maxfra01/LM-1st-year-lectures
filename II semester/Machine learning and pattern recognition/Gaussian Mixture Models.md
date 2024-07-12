# Introduction

We have seen that [[Gaussian classifiers]] assumes that class conditional distribution are Gaussian, but in many cases the assumption can be inaccurate:

![[Pasted image 20240606095136.png]]

Through **GMM** we can identify a family of distribution and perform regulation to get a good estimate of data distribution.
GMM can be employed not only in classification but in many tasks.

Let's consider again Gaussian classifier: we need to compute marginal density:
$$
f_{X}(x) = \sum_{c=1}^K f_{X|C}(x|c)P(C=c) = \sum_{c=1}^K \pi_{c} \cdot \mathcal{N}(x|\mu_{c}, \Sigma_{c})
$$
Where $K$ is the number of classes and $\pi_{c}$ the probabilities of each class.
In general this is an example of GMM: a Gaussian Mixture Model is **a density model obtained as a weighted combination of Gaussian**:
$$
f_{X}(x) = \sum_{c=1}^K w_{c} \, \mathcal{N}(x; \mu_{c}, \Sigma_{c})
$$
And we use the **notation**:
$$
X \sim GMM(M,\Sigma, w)
$$
Where each **distribution parameter is a vector of all gaussians means, covariances and weights**.

# Model

As we did with Gaussian classifiers, we consider a $D$ dataset generated by a GMM as **unlabeled**: we want to estimare the parameter of GMM through maximum likelihood:

![[Pasted image 20240606103858.png]]

A GMM can be interpreted as the marginal of a joint distribution of data points and corresponding clusters.
Although our training set cannot be well modeled by a Gaussian distribution, we can imagine that the set can be partitioned into subsets (**components or clusters**), in such a way that the distribution of the points of each component can be modeled by a Gaussian p.d.f.

![[Pasted image 20240606104102.png]]

If we knew each component we could estimate the parameter of each Gaussian, but in reality clusters are unknown: we treat clusters as **unobserved** random variables, so we need to estimate both cluster assignment and maximize the likelihood (marginal distribution)

We can compute **cluster posterior probabilities**:

![[Pasted image 20240606104601.png]]

Where $\gamma_{c,i}$ is called **responsibilities**.
At a first approximation we can assign a point to the cluster with the highest posterior probabilities:
$$
c_{i}^* = \arg \max_{c} P(C_{i}=c | X_{i} = x_{i})
$$
Now, given clusters associations we can treat them as if they were known class labels:

![[Pasted image 20240606113815.png]]

The first term corresponds to the log-likelihood of a multivariate gaussian classification model, where the class labels are assumed to be the estimated $c_{i}^*$
We can define mean and covariance for samples belonging to different clusters.

![[Pasted image 20240606114355.png]]

The second term is :

![[Pasted image 20240606114430.png]]

We could then obtain the updated set of model parameters $θ^{\text{new}} =(M^*, S^*, w^*)$
**We could iterate the process by computing new cluster assignments using $\theta^{\text{new}}$, and using the updated assignments to update once again the model parameters, stopping when some criterion is met**.

### Hard clusters problem

Consider data with hard cluster: a point is assigned to one and only one cluster: When posterior probability is equal to then we can't distinguish between clusters that generate the same sample:
$$
P(C_{i}=c_{1}|X_{i}=x_{i}) \approx P(C_{i}=c_{2}|X_{i}=x_{i})
$$
In the above scenario we don't know if $c_{1}$ or $c_{2}$ is responsible for the generation of sample $x_{i}$. This means that the algorithm we discussed is not maximizing the likelihood of the observed samples.

Let's consider that $\Sigma_{c} = I$ and $w_{c}=\frac{1}{K}$ then our cluster assignment is:
![[Pasted image 20240606115422.png]]
Since this decision is based only on the distance between the sample and the centroid of the cluster, this is exactly the **K-means clustering algorithm**_
- Compute the component or cluster $c_{i}^*$ whose centroid $\mu_{c}^*$ is closest to our point and assign $x_{i}$ to that cluster
- Re-estimate the cluster centroids from the given points, and iterate until convergence

We will see that a point is not completely associated to a single Gaussian component, but contributes to the estimation of different components according to its cluster (component) posterior probability.

![[Pasted image 20240606121659.png]]

We can interpret this expression as a weighted empirical mean where:
$$
N_{c} = \sum_{i}^N \gamma_{c,i} \quad \quad F_{c} = \sum_{i=1}^N \gamma_{c,i}x_{i}
$$
These two expressions are called **zero and first order statistics**.
For covariance matrix we can adopt a similar strategies and we get:

![[Pasted image 20240606123120.png]]

Since responsibilities are unknown we can follow the same procedure of hard assignment:
- Given $\theta$ (GMM parameters) we estimate the responsibilities for each sample of the dataset
- Given the responsibilities we re-estimate GMM parameters
This is known as **Expectatition-Maximization algorithm**.

### EM

As we shall see, the EM transforms the maximization of a log-likelihood log $f_{X}(x| \theta)$ into a sequence of optimizations of the jointlog-likelihood log $f_{X,H}(x, h|\theta)$ where H represents a latent (or hidden) random variable (or vector) i.e. a R.V. for which we do not have a realization (i.e. we did not observe its value)
