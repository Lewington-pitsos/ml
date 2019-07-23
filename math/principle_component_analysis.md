# Principle Component Analysis

## The problem

Sometimes when you approach a machine learning problem, you have a huge number of features given to you. This can be bad, because the more features you have the more complex your model must be, and 

1. Complex models are harder and slower to build
2. More importantly, complex models are more likely to overfit. 

I suppose `2.` is because every feature you have is bound to be a bit noisy, so there's a chance the model will incorporate that noise into it's reasoning. More features = more chances for this?? Maybe? 

## The solution: reduce the number of features.

There are two main processes for this.

1. Elimination: you remove some features. Presumably according to some strategy.

2. Extraction: you merge multiple features together and then only use the merged features. I suppose the idea here is that the noise of the original features cancels itself out when you combine them, and hopefully the signal remains. The idea is actually to make a very large number of these merged features (possibly more than the original features) and then eliminate most of them (using some clever strategy). This leaves us with *less* features than we started with, but since these features contain the data of many of the original features, hopefully without too much data loss.

Principle component Analysis is an *Extraction* method. One of it's benefits is that the resulting features are never super highly correlated (so linear models like them).

## Under the hood

 Basically, we have our original matrix `M` of values, we transpose it and multiply the two matrices to get `N`, the *covariance matrix* of `M` (which tells us, for each data point in each feature, how much it is correlated to each datapoint of each other feature.)