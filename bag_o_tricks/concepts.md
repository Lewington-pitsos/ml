# Concepts

These are tricks that take a bit to get your head around (so I can't just boil them down to a single line).

## Target Transformation

Ok, so you visualize your target variable just for kicks and you find that its distribution is slanted to one side. Say it's house sale prices, you see that there are many more data-points at the lower end of the scale. 

No biggie right?

Well, maybe. Let's imagine that the distribution is much worse. Let's say that, of the 10,000 odd data points, only 500 have a value above 300,000, and the rest are around 100,000. Very skewed as fuck distribution. 

Now, even assuming that these data points aren't anomalous, and can in fact be predicated accurately given your features, it's likely that any model you build is going to develop a natural bias toward predicting scores around 100,000. Like, it will naturally find that if it predicts a score near 100,000, it gets rewarded, so over time this is the way it will lean. 

Put another way, imagine you reduced your (skewed) dataset into two subsets `a` and `b`, both of which contain only 1000 values. Imagine `a` contains all 500 of the houses which sold for > 300,000, and `b` contains only 50 of these. Of these two subsets, `b` is arguably more representative of the superset, since its distribution matches that of the superset. 

Now imagine you create a (pretty good) model, and then train it `a` and `b` independently, resulting on two trained models, `ma` and `mb`. The theory is that `mb` will develop a bias toward predicting values ~100,000 completely independent of any features, whereas `ma` (whose targets were more normally distributed) can only rely on its features to make predictions, and so will be forced to pay more attention to them.

In short we would expect `mb` to perform badly when predicting the targets of `a`, since it's assuming a systemic bias which no longer exists, while `ma` should perform well when predicting the targets of `b`, since it uses the relationship between features and target (which naturally still exist in `b`) to make predictions. 

You would also probably expect `ma` to outperform `mb` when predicting targets on the superset. Even though the superset is also biased in the same way as `b`, a good understanding of the feature-target relationship is probably going to generate better predictions than a mediocre understanding plus a bias. I'm not entirely sure exactly why, but for one thing the bias is linear, so inherently it has little ability to model complex relations (so any model that relies heavily on a bias will be less able to make accurate predictions about a complex relation than a model which instead relies on a bunch of features, or puts more emphasis on a bunch of features or whatever).

TLDR: having a non-normalized distribution can create a bias in your model.

### Solution

Ok, so returning to our house problem where the distribution is just a little skewed, what can we do practically to prevent our model from developing a slight bias? 

It's almost certainly a bad idea to take out data until the distribution normalizes. Data is like gold dust, you don't want to just throw it out. 

Instead the idea is *target transformation* you should normalize the target variable(s). As in, actually transform their values until they fit a normal distribution, and then train like normal (with ONE gotcha which we'll get to later).

This seems counterintuitive. The most important part of the training data is the target, right? Like, the whole thing our model is geared towards is predicting target values, so if we *change* the target values before training, our predictions are just going to be *wrong* right? 

Essentially yes. But there are certain transformations you can do to the target that are easy to reverse later. For instance, imagine that for whatever reason we just multiply the target by 2 before starting training. We then train some models. We would expect the predictions of these models to end up being around twice what the correct values are. Like, if a model says a house is worth 200,000, the house in question is probably worth around 100,000, right? Our models are all wrong, bue they're *predictably* wrong. And we can probably get correct predictions just by dividing all predictions by 2, so in reality we haven't lost any predictive power at all. And maybe we gained something. Maybe our model, for some reason, trains faster when predicting higher values.

Ok, so in general, applying multiplications to the target won't result in any loss of predictive power, so if scaling the target in some way helps you, you should do it. Just remember to fucking UN-Scale any predictions (i.e. apply the reverse of whatever scaling operation you used) at the very very end of the process.

### Conclusion

So, finally: if you have a target distribution that *isn't* normal, to avoid biased models, scale the distribution into a normal distribution, do *all* your training, and then reverse the scaling for any predictions you generate.