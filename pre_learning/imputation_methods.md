# Imputation (filling in bad values)

Imputation is maybe not super important, and it's not super well studied. So for now we're just going off [this excellent paper](../papers/a-comparison-of-six-methods-for-missing-data-imputation-2155-6180-1000224.pdf), which basically just says the following:

*just replacing values with the mean of the dataset is bad, instead use FKN or bPCA*

## FKN: Fuzzy K-Nearest Neighbor
This seems to be the best performer, but it is quite expensive to compute. It's based on the more common `KNN: K-Nearest Neighbor` algorithm, so let's explain that first.

### KNN

This is a simple algorithm for predicting one of the values of a new data point, given a dataset of existing data points. 

Let's imagine you have a dataset of apples, and each data point just has two attributes (i.e. it's a 2D vector), weight, and height. You have many points, and you are now given a new data point whose weight is 5 but whose height is unknown.  

KNN says: 

1. Look at the existing data points which are most similar to the known data of the new data point. These are its "neighbors".
2. Find the average among the neighbors for the unknown value.
3. This is the unknown value for the new point.

Concretely you'd look for a bunch of existing data points whose weight is close to 5, average their height, and that's the height for the new apple.

How many neighbors do you look for? It depends. The KNN algorithm takes a parameter `k`, which determines how many neighbors to find. If `k` is 3, we find the 3 apples whose weight is closest to 5 and average their height. if `k` is 30, we find 30 apples. In practice `k` is never usually higher than like 7. 

The effect of `k` is: *higher `k` mean less vulnerability to random noise, but more computing time*. The expensive thing computationally is finding which data points are closest.

In addition, getting slightly more abstract, consider that if we set `k` to a the size of the *whole* dataset, we just end up finding the average of the whole dataset for every new point, which, well, there's no point, now we're just getting the mean. The whole idea behind KNN is that if two data points are similar in one way, they are likely to be similar in others as well. Apples with similar color, height and shape will probably have a similar width too. So, if you want to estimate an unknown value of a given data point, find other similar points and copy across. 

What does this have to do with `k` though?

Well, the more similar points the better, since more points means protection from noise, so you might want to choose a very high `k`. But then to fulfill that `k` you might end up straying too far from the original point, and the points you're looking at are no longer similar enough to give us good estimates (extreme example being straying until you've examined the whole dataset). So that's the trade-off you make with `k`: noise-resistance vs accuracy. The latter is very important, so `k` is usually quite small.

Naturally, KNN is very good for imputation.

### FKN

## bPCA: Bayesiean Principle Component Analysis.
This is very hard and mathsey. Basically it seems to perform a little worse than FKN, but is much less expensive computationally. 

## Multiple Imputation

Basically you aren't sure *what* the missing values are, so you impute your dataset in a number of plausible ways, resulting in a number of alternative datasets. You then train models on all the datasets and average the results of the models to get a good prediction. 

Ideally the different imputation methods should all be somewhat random and draw from (more or less) the same statistical distribution.

But like, fuck that. It's going to take forever.