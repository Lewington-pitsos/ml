# K-Means Algorithm

This is an algorithm that divides unlabelled data into clusters. You tell it how many clusters to look for, and then it identifies that many clusters and assigns each data point to one of those clusters.

## How it works.

Speaking abstractly, each cluster is defined by a *cluster center* and each data point is considered a member of the cluster with the closest cluster center.

We define the first cluster centers completely at random though. One for each required cluster.

We then use a process called "expectation maximization", which consists of:

1. an `E-step`, where we assign each data point to it's proper cluster (using the above method)
2. an `M-step` where, for each cluster we go through all its assigned points and calculate the mean point for that cluster, and finally reassign that cluster's cluster center to that mean point.

We repeat the E and M steps until "convergence" i.e. the iteration where the calculated mean point is pretty much exactly the cluster center already (to some predefined threshold of "pretty much").

## Is it good?

Generally speaking, people agree that each expectation maximization iteration *does* in fact bring us closer to a good estimate of the true clusters in the dataset.

However, the starting guesses can really fuck you up if they happen to land wrong. A good fix for this is just use multiple starting guesses (sklearn does this by default).

In addition, you need to tell it how many clusters you want. 

Plus, K-means always separates clusters in a linear manner, and always with respect to the *center* of each cluster, so K-means can only work with clusters that are more or less blobby. If a cluster is more of a long straight vertical line, K-means will either refuse to accept the ends of the line as part of the cluster, or it will basically draw a big circle around the whole line, and any point falling within that circle will also be considered part of the cluster, even if they're pretty horizontally distal.

This last is not the end of the world though. You can use the trick where you project non-linear data into a higher dimension, and then draw linear boundaries in the higher dimension that *can* separate the clusters. Sklearn has an option to do this.

Lastly K-means can be slow for large datasets, since the `M-Step`, requires us to iterate over every point.