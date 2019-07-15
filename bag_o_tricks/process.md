# Process for approaching a problem

1. get a shitty benchmark score (like "assume it will be same as before")
2. feed the RAW data into a learner to get baseline score

Keep doing the following till you beat the benchmark:

1.  transform the data (without adding features) in a sensible way
2.  feed into same learner and record score
3.  if the score is worse, or learning takes ages undo

Now you can do feature engineering

# Code Editing

1. set up good CV
2. engineer features based on it
3. save the features
4. use them to train a model
5. save the model
6. engineer features onto the training set 

# Leustagos

1. Understand the dataset. At least enough to build a consistent validation set.
2. Build a consistent validation set and test its relationship with the leaderboard score.
3. Build a very simple model.
4. Look for approaches used in similar competitions in the past.
5. Start feature engineering, step by step to create a strong model.
6. Think about ensembling, be it by creating alternate versions of the feature set or using different modeling techniques (xgb, rf, linear regression, neural nets, factorization machines, etc).




