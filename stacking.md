## Stacking

The general idea is: if you train like 4 models, some will have different advantages and disadvantages in different areas. You can average them, and if the advantages map to some underlying pattern and the disadvantages are largely random, you'll get a better score. But it would be nicer if we could identify where each model shines and pay more attention to it in those areas. That would probably leave us with a better score. 

Basically stacking is where we get a model to do this for us... approximately speaking at least. Intuitively, this positive effect will be greater where the models being stacked are disparate.

### Implementation

There are a few ways people use to actually do stacking. Here is a neat one though.

1. train model A on dataset D
2. train model B on dataset D
3. get A and B to make predictions `A_pred` and `B_pred` on D, the same dataset (*see note below on details*)
3. create dataset DD by adding `A_pred` and `B_pred` as new columns to D, 
4. train model C on DD

C in this case is a stacked model. woo. 

You can also train A and B on different feature sets of D and C on a third feature set, or maybe just the raw-ass features.

#### Note: preventing target leakage

In it's simplest form the above causes data leakage so be careful. 

Ok, so the idea is our stacking model will learn something about the relationship between `A_pred` and the target, something about the relationship between `B_pred` and the target, and using that knowledge, and both those features it will be able to make better predictions than either `A_pred` or `B_pred` individually.

All well and good, but recall that whenever we train a model M on some data D, M is likely to over-fit D. In other words, the relationship between M's predictions on D and M's predictions generally will probably not be the same. At the very least M's prediction on D are likely to be more accurate than M's predictions in general. 

So, if we trained a model A on data D, and then added `A_pred` to D, and then asked a stacking model to learn the relation between `A_pred` and the target of D, it would learn a relationship that doesn't accurately reflect the true relationship between the predictions of `A` and the true target generally (and in particular, the test set).

So, in order to stack properly in this way we need to make sure that if we train the model A on a given datapoint dp, we never then use A's prediction for dp when training the stacker. 

A straightforward way of doing this is something like K-fold cross prediction. So (in the case of 2 folds), you train A on half of D, make predictions on the other half, and then swap over halves. This way no prediction was made with the help of the target for that datapoint, and as a result the relationship between the prediction and the target won't be skewed by data leakage. 

### Usage

Generally stacking only produces small gains, which often in industry aren't worth the extra time and complexity. However, it *does* almost always produce at least small gains, so very useful on kaggle.


### Fun Conceptual thing

A lot of feature engineering could be thought of as stacking lol. You have a dataset of house prices and you generate a feature called `neighborhood_avg` which is the average price for a house in a given neighborhood. You've *kind* of actually just built a shit model. Kind of like a really bad KNN where you are trying to predict price by only paying attention to one feature (i.e. `neighborhood`). You've then added those model's predictions to the original dataset as `neighborhood_avg`. 