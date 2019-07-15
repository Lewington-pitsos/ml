# Entity Embedding

## Terminology

Discrete feature: a feature where every existing value represents a category. 
Continuous Feature: a feature where each value represents a quantity of something.
Ordinal feature: a discrete feature whose order has some meaning, e.g. the underlying category is "month of year".
Nominal feature: a discrete feature whose order may not mean anything, e.g. color.

## The Problem

High cardinality discrete features (ordinal or nominal). GBM's, linear classifiers and and neural networks are bad at dealing with these in their raw form.

1-hot encoding is often unfeasible, since it results in preposterously large datasets. Plus often the order of discrete features (even nominal ones) contains *some* meaning. Even if the feature is `item_id` it is likely that similar items will occupy contiguous ids, since that's the most natural way for someone to assign ids to items... and 1-hot encoding removes this information. Plus GBM's don't like 1-hot encoding.

## The Solution

Basically: Map discrete feature values (probably integers) onto a higher dimensional space (e.g. a grid) that situates values whose underlying categories are similar (spatially) close together. 

More technically we replace each discrete value with a vector.

