# Darius Barušauskas

Team up with a good person.

He spent 15 hours a day on this comp :'(.

Listen to Owen Zhang. 

- work on a single model for a very long time. Ideally you can get into top 30 with just one model.

-  sometimes tweaking data can be nice. For instance, you can imagine that you label encode some categorical feature, but the way you've encoded them, just by pure coincidence all the even features belong to one cluster and all the odd ones belong to another. It's going to be hell for XGBoost to separate them appropriately, since all it has at its disposal is the ability to divide into leaves using > and <. So: it's maybe a good idea to reshuffle your categorical features (e.g. by target ratio) before label encoding them. 

- Tree methods kind of sensitive to noise. They also get hurt by useless features. So: to feature selection is often important to boost performance. It is often just as effective as stacking.

- One of his best competitions he de-anonomized some features to find the dataset's timestamp. He was tipped off because most of the features were floats so he assumed someoene had just taken like, an int and then scaled and normalized it or something. 

- Generally speaking, sorting by numerical features and then looking at the distribution of the target is a good idea for EDA.

- A good approach is just looking for "tricks" in the data. A single feature might put you at the top.