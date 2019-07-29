# Stanislav

Former #1.

## Winzorization

Set data above/below 1st/99th percentile to those percentiles. It can help

## Target TFM

log(target), log(target + 1). Is a form of regularization in and of itself. Goood for deep learning.

## Linear regression as feature selection

Imagine you have a huge-ass dataset. Truly immense. Huge number of samples, thousands of features. XGBoost is crying. What do you do?

Instead feed the data to a simple linear model (L1 regularized). Because of the L1 regularization, certain features will be considered more important by the linear model than others. Take the 1000 or so most important features and train as before. 

## External Training Data Bagging

Basic bagging strategy: 

1. divide train set into a few bags.
2. train a model on each bag
3. average the models for better results

External bagging:

1. divide train set into a few bags
2. train a model on each bag, plus `n` samples form outside that bag (i.e. "stolen" from other bags)
3. average the models

You work out `n` by trial and error using validation.


## Timeseries Bagging

In timeseries data, chronologically adjacent samples are generally similar. This can cause weird effects if you bag the dataset according to large chronological chunks. The solution is: create say 5 bags by putting every 5th sample in the first bag, the sample directly after every 5th sample in the second bag and so on.

## Linear models on non-linear dependencies

often the relation between a feature and the target is *not* linear, which makes linear models give poor target prediction. To overcome this you should manually alter the features till the relationships *are* linear. Examples are,

- the sin of a feature
- the abs of a feature - another feature
- binary features that depend on the value of the feature

 This will probably result in a lot *more* features than you had previously. It looks like you need to work out the relations yourself in your head though. A fun way to find some binary features though is to build a tree-based model, see what splits it made, and add those splits as features.

## Measure of similarity

Sometimes you need to work out how similar two features are. Something called `Normalized Compressed Distance` is generally a good measure, no matter what your features are.

## Isotonic regression

So the most straightforward non-linear model you could think of is just "draw a line however the fuck you like and that's our prediction". This is called a `free-form line`. Isotonic regression is just this, but you have one more constraint: the line must never decrease (i.e. a value for y must never be lower than the previous value of ), or possibly never increase depending on the problem. Some relationships might be well fit to such a line, like, say the total number of boxes labelled over time. You can even use it to fit relations that do decrease themselves sometimes, as long as there isn't much decreasing. So like, the summit height of a mountain while tectonics mean it's still rising overall. The height sometimes decreases, but your model might be better if it assumes that it never does.



