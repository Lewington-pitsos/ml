# TabNN: using neural networks for tabular data

Basically: deep learning with NN's seems not to perform very well on tabular data so far. This is probably because the areas where deep learning really shines have been carefully studied and the deep learning algorithms being used have been carefully tailored to their domain. E.g. image recognition uses the convolution thingy, because in images generally spatially close data points should be considered together.

Tabular data is a domain that has not been as carefully studied yet.

## Issues

We also know that just basic, fully-connected networks have difficulty handling tabular data because these are designed to look for very (mathematically) complex solutions, while tabular data often has much simpler solutions.

## Solutions

Two things are suggested to help deep learning people when doing tabular learning:

1. Explicitly tell your network what features to look at. Don't just give it every possible feature and be like "have at it".
2. Reduce model complexity. Since again usually the solutions tend to be simpler.

Basically, the strategy is to apply some tree-based model to your data, and use the results you get from this to help design a neural network, than you can then feed the data to to generate actual predictions.

## Steps

The process recommended can be broken down as follows:

1. Detect *groups of expressive features* in the data using a tree classifier
2. Reduce/condense these groups down further as much as possible
3. Build a NN using the groups from 2
4. Take the *structured knowledge* from the classification tree and transfer it to the NN

If you did all this correctly, you can now pass data into the NN and get accurate predictions.

### 1. Detecting Expressive features (Automatic Feature Grouping)

I'm not 100% but I can only imagine that by *expressive* the authors just mean *important* or *should weigh heavily when making predictions*. We want to find *groups* of features which, when taken together, have a lot of *expressive power* i.e. help us to make predictions a lot. 

Happily for us, classification trees find these automatically. Recall, how a classification tree is built:

1. you have a set
2. you use the features of that set to create lots of different "splits" in the original set (with each split creating two subsets)
3. you select the split which gives us the most information (i.e. "purity") gain
4. you repeat this process with the newly created subsets until you're happy

in step 3. by selecting a split we're actually selecting the most expressive feature for the current set. So, a whole tree contains group  of features (one per split) which, according to our tree building algorithm, are very expressive when taken together. This is exactly what we're looking for. 

So: to detect expressive features, all we need to do is create a whole bunch of classification trees from a dataset (using whatever algorithm, though the paper people recommend GBDT) and extract the features from their splits.

### Reduce Features By Reducing Feature Overlap Between Groups (Feature Group Reduction)

