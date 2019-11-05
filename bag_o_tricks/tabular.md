# Confirmed

- Never just run a single valdiaiton round, 5-fold cross validation at least. 

- drop the FUCKing id column (~1%)

- tune parameters on the same random seed or else your results might be partly due to rng (boo)

- target transformation, normalize the target variable, (~0.4%)

- if numerical features are skewed, try un-skewing them (box cox transform with lambda 0.15 is good), most helpful to linear classifiers (but oddly still helps tree classifiers).

- when imputing data, calculate things like mean, mode, etc from the **combined** train/test dataset. These values will be more accurate period.

- simple feature aggregates for all occations:
    - the frequency of an important feature
    - the "concat" of two important-looking features (i.e. convert to strings, then string concat)

- if 3 or 4 features are very high xgboost importance try looking at their relations manually. Maybe you can find why xgboost likes them.

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

