# How to know which features to turf

## Let the model do it

You can simply add a penalty to your model for every feature is uses. The model will naturally try to drop features where it can, and you end up with a model that depends on less features,

Most models will have a paremeter for this.

### issues

- The model starts training on all the features so it can take a long time. 
- The model might drop too many features if the penalty is too high, and you end up with a worse model.

## Score each parameter with respect to the target

There are two good mathematical tests that compare each feature to the target and give each a score. They are

- chi-squared Test, indicates whether there is a relationship between the feature and the target.
- f test

These are available in sklearn. 

Sklearn also has a SelectKBest function that is helpful.

## Ask Lasso

By nature Lasso regression weeds out parameters it has trouble using helpfully. So learn the set using lasso and only keep the features that Lasso kept.