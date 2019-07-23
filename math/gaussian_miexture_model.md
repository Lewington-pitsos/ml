# Gaussian Mixture Model

GMM's can be thought of as an improvement over K-means.

One of the issues with K-means is that each point assigned to a cluster is treated as being equal in it's membership of that cluster. In reality though, some points are always more obviously cluster members than others. E.g. the points very close to the cluster center can safely be seen as having a very solid/confirmed membership, while points very far away from the cluster center, or close(ish) to another cluster's cluster center, should probably be considered to have a more tenuous membership status.

K-means just classifies points as either inside or outside a cluster in a binary fashion.

There's also the circular cluster issue, if you recall. 

Both these problems are worse in low-dimensionality data sets. 

## Solution

Gaussian Mixture models solve these two problems specifically. They use a different `expectation maximization ` procedure. GMM's still require you to choose the number of clusters beforehand.

1. Choose the `location` and `shape` of each cluster randomly.
2. `E-step` for each point, calculate the probability of its membership in *each* cluster. 
3. `M-step` use these probabilities to update the `location` `shape` and `normalization` of each cluster.


The main thing to take away from this is that each cluster is no longer a hard, circular in/out boundary. Instead it's a field of probability values that decrease as we move further away from the cluster's center. These probabilities are assumed to be gaussian.

## Distribution

GMM can be used as a clustering algorithm, but keep in mind that it actually generates data point probabilities for each cluster. This means we can use it for other purposes too. For instance, once you have a GMM for a given dataset, you can generate "matching" datasets i.e. datasets which also fit that same GMM. If the GMM is accurate, these new datasets should be distributed in a similar way to the original.

In fact, GMM is probably better as a dataset "density estimator" than as a clustering algorithm.