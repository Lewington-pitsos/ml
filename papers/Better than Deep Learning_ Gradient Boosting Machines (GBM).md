# Better than Deep Learning_ Gradient Boosting Machines (GBM) (Szliard Pafka)

Why machine learning is not that great:

Deep learning is very good at certain things (vision, text generation, etx). It is also good where you have crazy amounts of crazy data (e.g. games, where you can generate fucktons of data just by making the machines play each other). In industry though, where data is often limited, the guy from the video has a hard time getting better results with deep learning than with more traditional stuff.

Fun fact: XGBoost 2/3 kaggle competitions won with XGBoost, only 1/3 won with deep learning (as of 2016).

main advice: 

1. if you have large-ish dataset of tabular data, try boosting *first*, 
2. if you have small tabular dataset (>1000), use linear model (or else you will likely overfit)
3. huuuuuge data, NN and boosting become infeasable since you have to train for days. Instead use something light, like a linear model
4. image, speech and text: use deep learning
5. OR, try them all *dabs wildly *.

## How to do boosting good

You can improve results a lot by:

    1. data cleaning
    2. feature extraction

In fact, feature extraction and data cleaning are often *more* important than which algorithm you choose. I.e. good pre-processing/feature engineering with any type of model is better than the best model with average feature/cleaning stuff. 

### recommended packages

Generally (advice from super good kaggle guy), look for open source libraries that *already exist* and and use them for your own evil ends. Then you can spend your actual time feature engineering.

H20 ai, XGBoost and LightGBM seem to be the best. They are all in R or python.

Fun fact though: you can't really parallallize gradient boosting, it has to run sequentially right? So there's not much point trying to do it on a GPU. GPU is only helpful for deep learning. 

### important hyperparameters

1. n_trees "couple of hundred"
2. learning_rate 
3. max_depth "between 5 and 15"
4. early_stopping (prevents continued training after over-fitting begins somehow)

When searching for hyperparametrs don't make them into a grid layout, it's more efficient to make a random layout