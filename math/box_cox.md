# Box Cox

This is a function you can apply to a skewed distribution in order to make it a bit more normal. It is a bit more complicated than a simple `log`. This is what it is 

#### Legend:
- `f`: the box-cox function
- `l`/`lambda`: a parameter to the box-cox function
- `y` an input to the box-cox function


    fl(y) = 
        if l == 0: log(y)
        else: (y^l - 1) / l

With the idea being, you try lots of different `l`'s until you find one that happens to make *the current distribution you happen to be working with* as normal as you need. `l` is very situational, and you probably work it out using gradient descent. 

Anyway to be a little more concrete, if `l` is 2, we get:

    f(y) = (y^2 - 1) / 2 

Not so difficult right? 

## What does it do, in laymans terms?

Box-cox takes every value in your distribution/set/bunch and shifts them all in one direction. *but* it shifts some values more than others (still in the same direction) *and* preserves the order of all values.

The extent to which values are shifted, and the direction they are shifted in depends on `l`.

For `l = 1`, the box-cox is just a linear transform: all values have 1 subtracted from them. This won't actually make the distribution more normal, so you wouldn't use this unless you're making a mistake probably.

For `l > 1` higher values are increased a lot and lower values are increased a little. This effect gets more intense the higher `l` is. If you think about the values being in a distribution, it has the effect of shifting the distribution *generally* to the left, but moving higher values much further to the left, creating a tail trailing out to the left. Of course, if you happen to have a cliff on the right side of your distribution, this is probably exactly what you want.

for `l < 1` the exact opposite: every value is decreased, but larger values are decreased more. This would shift a distribution generally to the right, and create a right-leaining tail. 
