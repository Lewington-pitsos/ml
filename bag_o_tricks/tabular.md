# Confirmed

- Replace null values with the existing *average* or *median* for that column. That way these values aren't allowed to fuck with the statistics of the column *and* they probably also better approximate the actual value that should be there.

- K-fold cross validation

- drop the FUCKing id column (~1%)

- tune parameters on the same random seed or else your results might be partly due to rng (boo)

- *fine tune* even more once you get close to desired parameters (~0.5%)

- a dataset containing many dubious features will perform better than a small subset of that dataset containing only the most correlated features. Dubious data appears to be better than none at all. Hence high-quantity feature engineering is maybe good.

- target transformation, normalize the target variable, (~0.4%)

- when imputing data, calculate mean, mode, etc from the combined train/test dataset. These values will be more accurate period.

- if numerical features are skewed, try un-skewing them (box cox transform with lambda 0.15 is good), most helpful to linear classifiers.

- impute data using k-nearest neighbor (~0.01%) 

- kill outliers

- use 1-hot encoding vs label encoding (use the latter when there is some inherent order, otherwise use 1-hot)

# To Try:

- impute using fuzzy knn

- combine the predictions of multiple models for best results.

- Bayesiean Principle Component Analysis

# Dubious

- auto-generating lots of dubious features is helpful

- intuitive feature engineering is sometimes required

