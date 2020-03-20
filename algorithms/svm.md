# Support Vector Machine

In classification you want to draw lines between different groups. 

Cool, so lets say you have two groups. You pick some line that divides these two, and set this as the boundary line. You categorize new observations using this line.

BUT: in reality, some lines will be worse than others. A line very close to one group and far away from the other is probably not a good line, for example. It might lead you to classify new samples with the former category that are much closer to and hence probably belong to the latter. 

So: how do we choose the best dividing line?

## How to draw the line

Common sense says "just draw a line equidistant form both groups". 

But what does "equidistant" mean? Are we talking distance from the center of each group or the nearest edge or what?

Basically maths people like using the group edges. This kind of makes intuitive sense too, since we generally consider observations closer to the group's center than the furthest observation in that group to also be in that group.

According to the SVM idea the way to draw the line is:

1. find the member of group A that is closest to any member of group B (a')
2. find the member of group B that is closest to any member of group A (b')
3. find the point equidistant between a' and b': p'
4. draw your line through p'. 

The `margin` of p' is the shorter of the distance between p' and a' and the distance between p' and b'. So, if p' is exactly equidistant to a' and b' the margin is the distance to either. If either is closer, the closer distance becomes the margin.

A classifier whose line is equidistant from a' and b' is called a `maximal margin classifier`. These are generally shit though, since one group usually has some outliers that you want to give less weight to. Often you'll want to ignore outliers. 


`Soft margin` is the name given to the margin of a classifier that ignores some outliers. 

A `Support Vector classifier` is a classifier that uses soft margins. also called a `Soft margin classifier`. The margin lines (lines drawn along the nearest observations from each group) are called `support vectors`.

## How to work out which observations to ignore

Basically the answer seems to be: use cross validation. Draw random margins until you get some that perform well. These will probably be soft margins i.e. in practice you will probably ignore at least some datapoints.


## Limitations

SVCs are strictly linear classifiers. 

## Support Vector Machines

These handle nonlinear problems better.

Essentially the way SVMs work is:

1. start with low dimensionality data that cannot be classified linearly. 
2. Increase its dimensionality
3. create a SVC to apply to this higher dimensionality data and classify new observations using it.

### How do we increase dimensionality?

We use something called a `kernel function`. A simple as fuck version of this is the `polynomial kernel`. This function takes a parameter `p`, and creates a new column/axis of the data equal to each observation ** `p`.

i.e. polynomial_kernel(1) will not create any new axes because any number ** `p` just equals that number. 

polynomial_kernel (2) creates a new axis where the value of each observation is that observation ** 2. 

We work out a good value for `p` using cross validation. If you have a SVM that is trying to solve a super nonlinear problem and you give it the polynomial kernel, it should end up assigning a high `p` value to it.

There are lots of other kernels though. `RBF` is a good one.

### Kernel trick

In reality transforming data into higher dimensions is expensive. So clever maths people do some clever maths that allows them to compare normal, low dimension observations *as if* those observations were in a higher dimension. This is called the `kernel trick`. 