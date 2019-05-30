# Gradient Boosting

It's important and based on *trees*.

## Classification Trees

These are basically flowcharts. 

You start with your whole sample `s` of data points. You then split `s` into two subsets `s1` and `s2` by taking a simple predicate and grouping the members of `s` depending on whether they pass it or not. For instance, if `s` is just a set of real numbers, you could split `s` using the predicate *is greater than 1*. Having done this we are left with `s1` and `s2`. 

What we just did is a very simple example of a *classification tree*. It has a single root node, `s` and two branches, `s1` and `s2`. Naturally we could make it more complex by splitting `s1` (or `s2`) further using another predicate, and further still by making further subsets from the resulting subsets of `s1`, and so on.

### Ok, cool, So what?

Classification trees can be used to make predictions.

Let's say that average value of `s` is 2.5. 

Now, if someone told me, let's imagine, that "oh, hey, there's one value `v` that belongs in `s` that you missed". And I'm like "ok cool, what's `v`" and they're like "no bitch, im not doing your work for you, you'll have to guess what it is". This, naturally would make me sad, but I'd probably keep a stiff upper lip and guess that `v` has the value of 2.5, since that seems to be the average value in `s`. I'll probably be wrong, but assuming `s` isn't entirely random, there's a good chance im not off by too much.

Ok, now let's imagine I still don't know the value of `v`, but I do know that `v` is less than or equal to 1. I can't guess 2.5 anymore because I know that guess is wrong. But my analysis so far tells me nothing about the values in `s` that are less than or equal to 1. So all I can really do to determine `v`'s value is choose a random number below 1, but this isn't a very good solution.

BUT! Remember, we already just earlier built a tiny classification tree for `s`, maybe it can help us. Recall that one of our branches, `s2`, contained all members of `s` that are less than or equal to 1. If we find the average of `s2` (let's say literally just 1) we can make a better guess at `v`'s value (namely 1). 

Tada! our tree helped us make a prediction. 

### That doesn't seem like a very accurate prediction.

Intuitively it doesn't seem like it! But our tree was very simple and `s` could have been pretty much any set. 

Let's get a bit more concrete and imagine a product, say *bob's handwash* that always ships in boxes of 1, or 4. Now let's imagine a store clerk in some store somewhere was making an inventory of all *bob's handwash* in stock. He would go up to all the boxes of *bob's handwash* in the store, and mark down the number of units in each box on his notepad. Now let's imagine that all along, `s` was just the numbers on the notepad. 

Now we can see that the tree we build from `s` is actually quite useful. Because the tree we built maps onto the underlying structure of `s`, we can make accurate predictions about what other numbers might appear in `s` using the average values of `s1` (members of `s` greater than 1), and `s2` (less than or equal to 1). I.e. if the new member `n` is greater than 1, I can be pretty confident that it will be a 4, and otherwise it will be a 1. Before I made the tree I would have had to guess `2.5` in either case, which would also have been wrong in either case.

#### very contrived

Yes, but you can imagine less contrived examples im sure. At the very least you can see how making a classification tree from a set can get you closer to being able to predict new members of that set.

### but what if all I know is that the new member is a member of `s`?

Then yes, the tree won't help you. But usually in the real world you work out what qualities the new member has and *then* build a tree from your set to target members of that type. If you had no extra qualities of the member (other than it was a member of `s`), you wouldn't bother drawing the tree in the first place.

For instance, something you might *actually* want to predict might be "if a buy a green apple, rather than a red one, will it contain a worm?". In this case you take your set, (apples) draw a tree splitting it between red and green, work out the worm-propensity of the green branch, and boom, you have your prediction.

### Again, that doesn't seem super useful

Nah, it is though. Like, imagine you have 500 years of really detailed apple data. Someone gives you an apple and asks you whether it contains a worm. if you just stare at the data you probably won't make a good prediction. But if you make a tree and keep subdividing your data using the qualities of the apple you've been given (color, breed, size, weight etc), you'll end up with a smaller subset of historical apple examples from which you can probably make a much better prediction.

### Ok fine.

## Decision Boundaries

You can also use classification trees to make *decision boundaries* (basically, divide your data into sub classes). 

So taking the apple data, we can break all new apples we are given into `wormey` and `worm free` by looking at our data and creating a huge tree, finding the most specific limb for each new apple and counting the number of wormey apples in it. If the count is low we can chuck it in `worm free`, otherwise we put it in `wormey`. If we make the tree detailed enough, we might be able to ensure that each limb only contains apples of one or the other kind.

### good for you.

## Bad news though, trees aren't actually good

In practice trees aren't good at making predictions where the data is highly variable. So, in reality whether an apple contains a worm is probably hard to detect using the visible qualities of an apple. So what will either end up happening is the tree you build from your apple data will be too *bushy* (too many branches) or too *stunted* (not enough branches).

### What do those mean?

#### bushy

We have 500 years of apple data, but we're having trouble finding limbs that contain only wormey or non-wormey apples. So we keep splitting down and down and down until we have very tiny subsets of apples. Maybe with only 50 data points in each subset. Finally all our limbs contain only one class of apple, but by this point the subsets are so small that their wormeyness might be determined by random chance.

I.e. It's possible that all 50 of the small, round, red, lumpy, spanish apples form our dataset had worms in them. We would then classify new apples of that limb as `wormey`. In reality though, nothing about those apples makes them in any way prone to worms, it's just a coincidence. 

#### stunted

Ok so splitting too much didn't work, let's leave our classes general and our subsets large. Great, we stop splitting early and end up with like 30 limbs, each with 100,000+ data points (i.e. apples). The only issue is that for each of these limbs, around 0.2% of the apples contain worms, none of the limbs have many more or less worms. This being the case, we're not much better off than we were just with the whole big set of apples.

### soooo.. this was all pointless?

No, there are some things you can do to make trees better.

Also though, keep in mind that usually when creating classification trees you grow them without knowing the predicates beforehand. Instead you find all the predicates you could use to split a given set and choose the one which generates the least *impurity* (i.e. lowest chance of mislabelling any of the members of the subsets) and keep doing that until you reach some purity threshold. 

## Bagging

This is a method for reducing random noise in your data. You have a set of predicates `p` from which you want to want to grow a tree (i.e. with the apple data, someone gives you an apple and you have to predict something about it). Rather than simply growing one tree and looking at the appropriate limb, you randomly take `N` data points (apples) from the original dataset, build a tree from that and generate a prediction `q1`. Then you do this like, a billion more times, each with a new random `N` from the dataset, and generating a new prediction `qn` (which will probably be in the form of a probability, i.e. likelyhood of wormyness). Finally, you average all the `qn`'s to get `qz`, which is your final prediction.

This should be less random than simply sampling the whole of your original dataset.

## Random forest

Very similar to bagging except a smart dude worked out that if all the trees you grow during bagging are use pretty much the same predicates, well, fuck, they're not helping much are they? Still better than one tree, but if most of the trees look very similar, you're wasting a lot of computing power. 

Soloution: each time you grow a tree, limit randomly the predicates that that tree is allowed to use when growing itself. This way you get very different looking trees using very different predicates. The average of these very different trees will be even more robust to variance in the original dataset.

less predicates mean less correlation between trees.

## Boosting

One final, more effective approach. It involves gradient updates (kind of).

The steps of gradient boosting are:

1. Create a classification tree from the data set `s` like you normally would
    
    This tree reduces to a function `f`. It is highly likely that this function will not fit `s` properly. For each member `m` of `s` find the extent to which `f` is incorrect about `m`. This is called the *residual error*. So if `f` predicts `m` should be 2.4 and `m` is really 6, the residual error for `m` is 3.6. 

    Each member of `s` will have a residual error. Together these residuals form a new set `s1`. 

2. Create a classification tree from `s2`, again in the usual way

    So exactly the same as the above step, except with the derived, residual data. You end up with a new function `f2`. No need to calculate residuals of residuals.

3. Finally, literally just add these two functions together into a new function `f3` (so for an input `i` the output of `f3` is `f(i)` + `f2(i)`). 

4. Aaaand iterate, go back to `s` and calculate the residual error of `f3(s)`, then repeat steps 2. and 3. Eventually you'll end up with a decent predictor.

### Why does this work?

The intuition is that you make a shitty model, then you work out a rough picture of the ways in which it is shitty (i.e. residual error set, `s2`), then you create a function that models that shittyness, and finally you offset the original model by this new model. Ideally the resulting third model should be a bit less shitty than the first model.

### Loss functions

Exponential loss function has the model generate probabilities and scores on how far off these are from ground truth (as opposed to making the model spit out an explicit prediction). This is nice because it means that a score of 0.9 for a value that is 1 on ground truth generates less loss than a score of 0.7 or whatever, which intuitively it should.

I imagine this means that model gets rewarded as it gets closer to predicting correctly, which probably helps it understand the underlying structure of the data better than if it only gets rewarded for correct predictions. Also I suppose the loss function is "richer" this way since it is taking a more nuanced survey of the model's predictions.

### Tree size

Weirdly very stumpy trees can give the best performance sometimes. Trees with a single split, if there's enough of them, have been known to perform the best on some problems. 

Not all problems though.


### Shit gets cray

Think about this though: boosting doesn't actually need to use trees. The principle is: (1) create a shitty function approximating what you want from `x` (2) find the (exact) ways and extent to which it is shitty, let's call this `y` (3) create a similarly shitty function approximating `y` from `x` (4) add this new function to the the original shitty function to get a new, slightly less shitty function. Repeat.

Any shitty function will do, tree classifiers are just a good candidate cuz they're cheap.

I don't know if anyone actually uses this fact, but it's good to remember.