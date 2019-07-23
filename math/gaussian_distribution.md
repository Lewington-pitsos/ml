# Gaussian/Normal Distribution

A gaussian distribution is often represented as a function

    p(x)

Conceptually speaking, `p(x)` takes any real number and returns the *probability* of that number. 

Concretely `p(x)` is strictly defined, using two parameters `sig` and `mue`, and 2 constants `pie` and `e` as:

    1 / (
        sig * sqrt(2 * pie)
    ) * exp(
        - ((x - mue)**2 / (2 * sig) **2) 
    )

Don't even bother trying to understand that. The important thing is that you need a `sig` and a `mue` in order to create a gaussian function. Conceptually `sig` is the `standard deviation` of a dataset, while `mue` is the average of a dataset.

The big idea is that every dataset (in real life) shares some characteristics. More specifically generally there will be less samples as the values of those samples attributes move away from the dataset's mean. 

Given this, if you have the mean of the value of an attribute in a dataset, you can kind of construct a fake dataset by picking values at random, but with a lower probability as those values move away from the mean. 

Kind of make sense? There's something similar with standard deviation, i.e. the values of an attribute in a dataset generally tend to have a specific relationship with the standard deviation of that dataset, so you can make an even better fake dataset by taking the SD into account too when picking random values. 

And so that's what `p(x)` is: a function that takes a mean and SD and using these, maps every real number to a probability of it being in the dataset. You could then use this to make a dummy dataset if you wanted.