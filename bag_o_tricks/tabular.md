# Confirmed

- K-fold cross validation

- drop the FUCKing id column (~1%)

- tune parameters on the same random seed or else your results might be partly due to rng (boo)

- *fine tune* even more once you get close to desired parameters (~0.5%)

- a dataset containing many so/so features will perform better than a small subset of that dataset containing only the most correlated features. Hence high-quantity feature engineering is maybe good.

- target transformation, normalize the target variable, (~0.4%)

- if numerical features are skewed, try un-skewing them (box cox transform with lambda 0.15 is good), most helpful to linear classifiers (but oddly still helps tree classifiers).

- when imputing data, calculate things like mean, mode, etc from the **combined** train/test dataset. These values will be more accurate period.

- impute data using k-nearest neighbor (~0.01%) 

- removing *some* useless columns can improve performance

- kill outliers

- use 1-hot encoding vs label encoding (use the latter when there is some inherent order, otherwise use 1-hot)

# To Try:

- use feature engineering to make the relation between all your end features and your target linear, and then use a linear classifier.

- if the task specifies values to fall within a certain range, make sure that the values you are using for validation also fall within that range. OR add a `y_range` to your loss function.

- find variables that are highly correlated with each other (something like 0.8+) and drop the one that is less correlated with the target

- impute using fuzzy knn

- Bayesiean Principle Component Analysis

# Dubious

- make the bad_val indicator variable columns according to jeremy

- auto-generating lots of dubious features is helpful??

