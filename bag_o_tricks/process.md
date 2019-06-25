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