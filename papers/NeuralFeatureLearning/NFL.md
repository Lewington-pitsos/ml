# Neural Feature Learning

A big part of working with relational data is something called *feature engeneering*. It sucks and is time consuming, but you need it for state-of-the-art results (at least with XGBoost). This paper proposes a way to learn features automatically using Neural Networks.

## Feature Engeneering

You have tabular Data of horse race results. Each race has the race venue, horse name, and starting price. You have to predict winners. 

Now, if you're lazy you might just chuck this table into XGBoost and see what pops out. But if it's really important to predict these winners, what you might want to do is try to extrapolate additional data from the existing data. For instance, the data you have does not say the state of any given runner. But, you *do* know the race venue for every runner. So you can look up which state each venue is in, and then add an extra "state" column (or *feature*) to every row in your table. Now you have more data to work with, and if the state is in any way correlated with certain runners winning, your XGBoost might pick the relation up and you'll end up with better predictions.

This is called *feature engineering*. You just engineered an extra state feature into your dataset. Congrats.

We can see how this would be quite tedious and time consuming though.

## The aim in more detail

The paper wants to create a general algorithm for auto-engineering features across all domains.

### how tho?

Basically neural networks. 

More precisely: relational data is interconnected. I.e. entities from table `dogs` often relate to (are connected to) entities from table `people` (owners). One could think of certain strings of these connections as trees. For instance, imagine the dog `fido` is linked to two different owners, `bob` and `maggie`. These links could be thought of as a very stumpy tree with only two leaves with it's root at the `fido` row. Of course there would be other links in the database. Perhaps each `people` entity is linked to a `hat` entity, and a `lunch` entity. We could use this information to construct more substantial "trees", perhaps from `fido`, through his related `people` and through to the eight `hat` entities linked to `bob` and `maggie` collectively. Or through to their lunches. Or we could make "trees" from `bob` back the other way to all the `dog` entities he is related to.

Point is: we can think of this linked data as forming a whole array of different linking trees running in different directions.

And basically the idea is, we can collect information about entity `x` by generating random linking trees from it, and then gathering data about those trees. For instance, we might make a tree from `fido` to his owner's `hat` entities and simply count the number of leaves (hats) on that tree, we might learn that fido is probably quite well fed (since his many-hatted owners are probably quite well off).