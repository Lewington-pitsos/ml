# #1 Kaggler 2015 Advice

## Process

### If you keep iterating against the public leaderboard you can overfit it. 

This is unfortunate, since we don't really have any other way of testing how well we're doing. The other thing we have I guess is our own brain. So if something works on the LB and our brain is skeptical, maybe bin it. If you can find non-public LB ways of motivating your behavior, use them. 

The temptation is very strong though. 

### Learn from others.

There is a temptation to try to do everything by yourself, but everyone else together is always going to be smarter than you. Don't try to beat them all for christs sake.

### Consciously allocate time to different tasks.

So much time for parameater tuning, so much for feature engineering, etc.

## Technical

### GBM is very good in most circumstances.

It can:
    - capture non-linear functions
    - find complex interactions
    - treats missing value nicely

Hyperparameter values:
    - **number of observations in leaf:** You can work this out intuitively kind of, just ask "how many samples would I need in a leaf for it's average to be representative for this dataset?". Noisy datasets require larger leaves.

Issues:
**High cardinality categorical features**: **FOR SOME REASON** tree based models perform badly on these. You can't 1-hot encode them or else fitting will take forever. You want to reduce cardinality somehow. 

One method is to give your high cardinality features to a linear model (like *ridge regression*) and have it predict the target from just that. This model will be shit. However, it's outputs contain a fair amount of information about its inputs, while also being lower cardinality. So you then feed the linear model's predictions to your GBM *in place* of the original feature. 

**WARNING**: The better the linear model is the more weight your GBM will give it. If your linear model starts over-fitting, it's encoding-predictions become less valid *and* the GBM pays more attention to them, which is a recipe for disaster. Also because you are passing the same data through 2 models, the relationship you are mapping between that data and the target will become like, twice as fit/precise/strict as usual, so the chance of over-fitting becomes quite high. 

To avoid this: use *different* data for the linear and GMB models. Split it in half or whatever. OR if you are strapped for data, split the data into K folds, and build K models, each only trained on a single partition. These form an ensemble, which makes predictions. But it does it in a funny way. When we want to predict a value for, say `data sample 1` we first find the partition it belongs to, say partition 3, and then we drop  model 3 (the model trained on that partition), and make a prediction using the *other* models. That way the prediction for a value never comes from a model with access to that model's ground truth target.

Improving GBM
Keep in mind that GBM can only *approximate* nonlinearities and complex interactions. It can't model them exactly. So one way to improve a GBM, if you suspect that the relationship between the data and the target contains nonlinearities and/or complex interactions is to use feature engineering to encode these explicitly into the data, and then feed this augmented data (whose relation to the target is now linear/simple) to the GBM.

Also: the way GMB discovers features is literally just greedily trying every possible relation/function/linearity. The chances of it finding any *given* relation are quite slim. So, if you know of some specific relation that is super important (i.e. previous month's sales are a very good indicator of next month's sales), you should, again, use feature engineering to encode it. 

### Generalized Linear Models

Glmnet in specific is good.

Linear models are kind of the *opposite* of GBM, because GBM expresses relations in terms of non-linear "steps", whereas linear models always express relations as lines. This means they can complement each other nicely. Very nice to put in an ensemble with a GBM.

Lienear models are good when
- you have a huge ("billions and billions") amount of data. Processing speed. 
- you don't have enough data. Non-linear relations are hard to find. If you don't have enough data you can easily miss most of them, pretty much no matter what method you use. In these cases you're better off just trying to  find linear relations.

On the downside, linear models are over-sentitive to:
- outliers, so you must take care to remove these before learning
- the distribution of data, so you need to scale variables before learning

You should use *always* use regularization when making linear models.

Generally you should use L1 regularization, because it assumes sparsity, and if the assumption is correct it helps a lot, and if it's wrong, it doesn't really hurt much.

## Ensemble

Ensembling (weighted average) always wins kaggle competitions.

The idea is that models are all wrong, but wrong in randomly different ways, so blending makes the wrongness cancel out.

Simple blending is usually good enough.

When ensembling, *diverse* weak models are better than similar strong models. Or, the dream scenario is like, one strong model and several (less weighted) different models to help it out. 

Ways to make models diverse:
- different modelling tools (GBM, Glmnet, SVR, etc)
- different feature subsets
- different observation subsamples

When building diverse models it isn't always a good idea to decide whether to keep or bin them based on the public LB score. This can be misleading unless the public LB dataset is large enough.

### Over-fitting trick

Ok, so think about this. Fred Nerks trains 4 models. Each one he over-fits to the public leader board. Each of these will perform shit on the private leaderboard. BUT, the assumption is that each will be shit in randomly different ways. So, when blended you will hopefully end up with a single model that is decent on the private leaderboard. This only works if there is a lot of data on the public LB set though.

## How to apply knowledge outside of kaggle

Things kaggle doesn't cover:
- selecting a problem
- collecting good data

## Example:

Encoding high cardinality categorical features.

Leave one out encoding.