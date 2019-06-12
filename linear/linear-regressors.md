# Linear Regressors

This is a simple subset of machine learning algorithm that always fit the training data using a (purely) linear model. They are fast to train, but often not super duper accurate.

## Simple Least Squares Linear Regression (OLS)

You have a dataset of 2 dimensional vectors. This is nice, because it means you can plot the thing on a 2d plane. Let's say each point is an apple, the x axis is its weight, and the y axis is its height. Imagine it. Great.

The aim of a linear regressor is to predict one of the values of any new data point, if given the other already. So ideally you build your classifier on a nice dataset, and from then on whenever you see an apple you can ask the regressor "hey I have an apple with height `h`, what's it's weight?", and the classifier will give you a *pretty good* approximate answer.

How does it do this? Well basically all it does is *draw a straight line* across that 2d plane we talked about earlier. When asked to predict the height of a new apple given it's width, it just looks at that line and tells you what the hight value is for the given width along that line. 

And actually, it draws a very specific straight line: the straight line that *minimizes the mean squared error* (`MSE`). 

### MSE

The MSE of a line is: the sum of the squared *difference* between every real data point value and the value that the line predicts. More formally, something like:

#### Legend:

- x: a real value
- y: a predicted value

    Sum(
        (x - y)^2
    ), for all x

Basically, the closer the line is to each data point, the lower the MSE.

Ok, so the goal of a simple linear regressor is to draw the line that *minimises* MSE. One way you could do this is by drawing lots of lines at different offsets and rotations and see which one works best for your dataset. Another way might be making a single guess-line and then using gradient descent to find the best one. Luckily though there's actually just some maths that you can do to work out the *exact* line. So making a simple linear regressor is kind of trivial, you just follow some steps. 

### Simple Linear Regressor conclusion:

Linear Regressors are very easy to build, but usually not that great. 

In particular they suffer from over-fitting a lot. This might seem counter-intuitive, since, when we imagine an OLS regressor in 2 dimensions we imagine something witha  lot of bias and not that much variance. Apparently though, as the regressor takes more and more parameters into account, it starts over-fitting. 

Plus, since MSE penalizes highly incorrect predictions a lot, simple linear regressors are quite sensitive to outliers. 

## Ridge Regressors

Ridge regressors are a little more complex. You can't create them trivially, and their main purpose seems to be to reduce over-fitting.

## Lasso Regressors

## Elasticnet

Elasticnet is just a combination of 