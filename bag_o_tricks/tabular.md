# Confirmed

- Replace null values with the existing *average* or *median* for that column. That way these values aren't allowed to fuck with the statistics of the column *and* they probably also better approximate the actual value that should be there.

- drop the FUCKing id column (~1%)

- tune parameters on the same random seed or else your results might be partly due to rng (boo)

- *fine tune* even more once you get close to desired parameters (~0.5%)

- K-fold cross validation

- a dataset condtaining many dubious features will perform better than a small subset of that dataset containing only the most correlated features. Dubious data appears to be better than none at all. Hence high-quantity feature engineering is maybe good.

- target transformation, normalize the target variable, (~0.4%)

- when imputing data, calculate mean, mode, etc from the combined train/test dataset. These values will be more accurate period.

# To Try:

- combine the predictions of multiple models for best results.

- use 1-hot encoding vs label encoding (use the latter when there is some inherent order, otherwise use 1-hot)

- if numerical features are skewed, try un-skewing them. This won't effect tree classifiers, but it will help linear classifiers. Use box cox transform.

# Dubious

- auto-generating lots of dubious features is helpful

- intuitive feature engineering is sometimes required

- kill outliers