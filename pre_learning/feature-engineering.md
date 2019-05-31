# Basic Feature Engineering Techniques

Basically a lot of it boils down to you being able to see underlying structure in the data that is important, but not explicit, and then making it explicit to your model before training.

tl:dr: when you're training a model don't just grab as much data as possible and shove it in. First you should finesse that data into as useful a shape as possible before giving it to your learner. This will make the learner's job easier.

## Indicator variables

There is an important binary class that isn't explicitly set out in the data, so you classify table entities manually using an `indicator variable`.

Let’s say you’re studying alcohol preferences by U.S. consumers and your dataset has an age feature. You can create an indicator variable for `age >= 21` to distinguish subjects who were over the legal drinking age.

Naturally you can create more complex indicator variables too. For instance, you have a huge list of houses and the people who live in them. Using this data you might be able to designate certain houses as belonging to say "hispanic neighborhoods", which will probably effect the prices of those houses.

**Note:** It can often be helpful to create indicator variables for "unknown" categories, otherwise your model won't know to treat these any differently to another known category of the same feature.

## Interaction Features

In certain cases some kind of interaction between two known features is actually super important. Naturally your learner should, in time, come to work this out for itself, but if you make the interaction explicit (by turning it into a feature in its own right) you're giving your model a head start.

e.g. maybe your dataset has a list of families with births and deaths recorded among other things. An important feature is probably *number of living children* which is an interaction between the `births` and `deaths` features (`births - deaths` is the specific interaction). 

Another example: You have the features `house_built_date` and `house_purchase_date`. You can take their difference to create the feature `house_age_at_purchase`.

People are currently working on ways to create both interaction features and indicator variables from a data-set automatically. They have had some success, but so far it still seems important to do this shit manually. Deep learning seems to be the best at this.

## Feature Representation

Sometimes the raw data contains the features you want, but the way they are represented might be kind of difficult to learn. 

For instance, you have a literal unix datestamp for each purchase. This is technically super rich, but not explicit about important features. You might want to extract the `week_day` and `hour` into their own features, and maybe remove the original `unix` column alltogether. 

Another example is **grouping sparse classes**. You have a categorical feature where besides the four or so big categories, you have like eight categories with ~4 samples each. The elements in these categories probably don't represent any underlying features of general elements in those categories (like, you probably can't generalize from 4 samples in most cases) so any effort the model puts into trying to find those underlying features is probably wasted, or even possibly detrimental if it leads to a bad generalization. So: basically prevent the model from trying to learn things about those categories by relabelling all those categories as `other` or something.

You have a dataset of people with a feature `birth country`. The dataset is like 1 million and there are ~4 people from a bunch of different countries. Just group all those countries as `other` so the model doesn't try to waste it's time by being like "AHA! All indonesians own blue cars!!". 

Another useful thing can be scaling down a feature. You might have `weight_grams` of a bunch of shipping containers. Your model probably won't get anything out of analyzing weight to that precise of a degree, so you might convert that feature to `weight_tons` instead and ignore the original column.

One final thing is to find say the average value of `time_in_transit` or what have you, and create a new column `transit_time_offset` which is `time_in_transit` minuts the average time in transit. This can be super useful.

## External Data (i.e. cheating)

Bringing in extra data to feed your model can help a lot. 

### Timestamped Data

Sometimes this is easier than you might guess. For instance, if your data is timestamped, you can grab fucking pretty much any other timestamped dataset relating to the same period and give it to your model too. Who knows, maybe you'll find some whacky correlation between `grain_price` from the agriculture dataset and `number_of_pens` in the office dataset. 

Sometimes the external timestamped data is more obviously useful though. Like, imagine you have some stock-market data, pretty much any other stock related data, or even financial data over the same or even a similar period is likely to contain some useful correlations.

### External APIs (i.e. blatant cheating)

You have some image recognition challenge, say lots of photos of swamps, and you have to find the images that contain people being attacked by crocodiles. Just fukken go over to Microsoft's Computer Vision API (which you can give an image, and it returns the number of human faces in it) and add that as a feature to your dataset. You can bet your model is going to have an easier time now eh?

### Geocoding 

See Timestamped data.

### Ask a domain expert

They probably know where to find data that is likely correlated to the problem you're trying to solve.

## Error analysis

You'll get errors. A very good thing to do is look at the kinds of errors you're getting. It's kind of hard to look at the model error as a whole though because there will be lots of datapoints. Here are some things to focus on though:
    
1. Look at the biggest errors
2. Break down error counts/size feature by feature
    
    Let's say one of your classes is very error-full. You can go back and create an indicator variable for that class to make your model focus on it more.

3. Unsupervised Clustering

    Run a clustering algorithm on the misclassified entities. You could turn these clusters directly into features, but this doesn't always work in practice. Instead it's maybe a good idea to use your human intuition to work out why these clusters are clustering like they are and create a feature based on that.


## Feature Importance Ranking

This is kind of just to help you reason about things, but it can be good to rank the original features by how "important" they are. This can inform the rest of the feature engineering process. One way to gauge "importance" is just checking the correlation between a feature and the independent variable. 

## Feature Extraction

Often your dataset will be crazy huge (think all NSA call recordings) and a lot of the data will not have any bearing on the problem at hand. *feature extraction* is the process of automatically extracting useful features from all this cruft that can then be fed into your model. 

There are a bunch of different algorithms for doing this.

## Feature Engineering for performance

Sometimes you can structure your features so cleverly that it allows you to use crazy models that usually don't work. For instance [Feature Engineering and Classifier Ensemble for KDD Cup 2010](http://pslcdatashop.org/KDDCup/workshop/papers/kdd2010ntu.pdf) is a badass paper where a bunch of chinamen reduced the complex features in the initial dataset to millions of simple binary features. This allowed them to use a simple linear model to do the actual predicting, which was awesome because linear models are performant as fuck. 

The initial data contained lots of non-linear relations, but those were feature-engineered away until you just had an immense dataset of binary features whose relationship to the target was more or less linear. 

How gacked is that?


## Some informative feature engineering papers:

[Round 1 Milestone Prize: How We Did It – Team Market Makers](https://foreverdata.org/1015/content/milestone1-2.pdf) - predict patients to enter hospital, used lots of sql


## Feature Overengineering

Question: Can feature engineering over-fit?

Answer: yes. You could, for instance, find that in your training set, feature `travel_time` is super important to the independent variable. "great" you say to yourself, I'll derive many new features from that feature and feed them in, and maybe ignore some other features. But guess what? Your data is skewed as fuck matey! Turns out `travel_time` isn't actually that important to the underlying structure.

So I suppose the important thing is: have some sort of validation procedure in place when working out which features are important.