# Texture Synthesis Using Convolutional Neural Networks


## Abstract

We took a CNN that was already trained to be good at classifying images, and used it to generate textures. It generated cool textures.


## Introduction

Texture synthesis means creating a function that will take an **example texture**, and produce lots of new, unique **sample texture**s from it. Sample textures are considered "better" the harder they are to distinguish (for humans) from the example textures used to generate them. There are two main approaches to texture synthesis.

### Non-Parametric approaches

The first approach is *Non-parametric* (i.e. not involving trainable weights). These methods usually work by finding clever ways to copy clusters of pixels from the example texture and arrange them so that the result looks nice to humans. These generally produce good sample textures. What these non-parametric methods *don't* do though is tell us much about what the example texture is like (does it involve curves? straight lines? certain colors or statistical properties?). 

### Statistical modelling approaches

An alternative approach to texture synthesis is to:

1. Grab your example texture
2. Collect a bunch of high-level statistics from it (e.g. color ratios or brightness)
3. Start generating random (or quasi-random) images and collect the same statistics from each image
4. Images whose collected statistics match those of the example texture are considered sample textures, or at least candidate sample textures

In theory images whose high level statistics match the example texture are probably visually quite similar texture-wise. The advantage of this approach is that in these statistics we're collecting we now have a kind of high-level, abstracted representation of the original example texture. We can study this abstraction and maybe learn something about naturally occurring textures.

### Enter Neural Networks

Neural networks are all about taking an input and collecting high-level data about it. Given this, we thought that Neural networks would probably be very good for implementing this second approach to texture synthesis. We went ahead and tested this hypothesis.


## The Network

We used a VGG-19 neural network that was pre-trained in object recognition. We made some very minor adjustments to it (see the actual paper for details if you're a nerd).


## How we did the texture synthesis

Broadly we did the following:

1. Passed the example texture, `e`, through our network, saving every feature map on every layer of that network.
2. Extracted some summary statistics `se` from each layer's feature maps.
3. Generated a random image, `i` (so like, random noise, looks like TV static).
4. Passed `i` through the network, saving every feature map on every layer again.
5. Extracted the same summary statistics from each layer's feature maps, giving us a second set of layer statistics `si`.
6. Compared `si` to `se`, and calculated a *loss* representing how different the two sets of statistics were. 
7. Performed a gradient update on `i` itself, in order to minimize this loss, and then repeated steps 4-7 until we got a loss of 0.

At the end of this process our `i` usually ended up being a high quality sample texture.

### Summary Statistics

For each layer, `l`, in the network we calculated how closely each of the separate feature maps on `l` were correlated. 

#### Spatial Data

All we cared about in this study was which features occurred together (or failed to occur together), we didn't pay any attention to the exact spatial locations where features occurred.

### Math version

For the nerds out there, the correlation between the feature maps for each layer was represented as a **gram matrix**.

For the rest of us, to get the measure of correlation between two feature maps, `f1` and `f2`, from the same layer (which would always be same-sized matrices), you:

1. Flatten them both into vectors (effectively throwing out most of the spatial information)
2. Multiply these together to get a third vector, `c`
3. Sum this third vector into a single scalar, `d` (throwing out the rest of the spatial information)

This process is called getting the **dot product** of `f1` and `f2`.

**Note:** `d` will be higher if `f1` and `f2` had higher numbers in the same spatial locations, and lower if, whenever `f1` had a high number `f2` had a low one and vise versa. This makes the dot product operator a pretty good approximation of the correlation between two same-sized matrices. 

**Also Note:** `d` isn't effected at all by exactly *where* the correlations occurred. If you jumbled up the activations in `f1` and `f2` randomly, but in exactly the same way, you'd end up with exactly the same `d`.

All we did is took every feature map in each layer, and found the dot product of that feature map against every other feature map in the same layer. The result for each layer would be a square matrix with as many rows and columns as the number of feature maps in that layer. A matrix made up of the dot products of other matrices like this is called a **gram matrix**.


### Loss function

We calculated the loss between the *example* texture summary statistics, `se`, (i.e. a gram matrix for each layer of our network) and the *sample* texture summary statistics, `si`,  as just the mean squared error (difference between averages of squares of each matrix). In more concrete terms:

1. For every gram matrix in `se`, multiply it by itself. 
2. Then calculate its average
3. Do the same for every gram matrix in `si`, you should now have two columns of averages
4. Calculate the total *difference* between these two (the sum of the absolute value of `se` - `si`)

That is the loss.



# End paper

The paper also included some interesting discussion and findings, but hopefully you now have a good idea of why ___ did what they did and how to replicate their experiment.

## Why gram matrices though?

One point I felt the paper glossed over a little was why exactly we might expect to get a good idea of an image's texture by passing that image through a neural network and then measuring correlations between feature maps in that network. Why feature correlations specifically? It took me a while to work it out myself so I thought I'd include a brief discussion here for anyone who's interested.

### Gram Matrices and feature correlations.

Recall that a feature map in a neural network is supposed to represent the presence of a given feature in the input. For instance, if a feature map, `f1`, from layer `l` is tracking the presence of red pixels, then high activations for `f1` in, say, the top right corner, mean that the input image had lots of red pixels near the top right corner. Naturally there will be other feature maps in `l` as well. For instance, there might be another feature map, `f2`, that tracks horizontal lines, and a third `f3` that tracks brightness.

What we were interested in for this study was the *correlation* between feature maps like this. Two feature maps, say `f1` and `f2` are considered highly correlated, if wherever `f1` has high activations, `f2` does too. The opposite is also true, `f1` and `f2` are poorly correlated if the two seldom have high activations in the same locations. In more concrete terms, checking the correlation between `f1` and `f2` should tell us how often the input image had horizontal lines wherever it had red pixels. 

And when you think about it, this is exactly the kind of question we would want to answer in order to describe *texture* of an image to another human. For instance I might try to describe the texture of this image:

![](./images/sky.jpg)

As a lot of small, fluffy white bits, and a lot large of smooth blue bits, which is pretty much just talking about the correlation between features ("fulffy" and "white" being two highly correlated features).

The gram matrix of `l` will be a matrix containing the sums of the dot products of every single feature map in `l`, against every other feature map in `l`. The sum of the dot product between two features should be a good indication of how highly correlated those two features are. So ultimately the gram matrix of `l` should tell us how highly correlated every feature in `l` was to every other feature in `l`. That's why gram matrices are so popular in texture synthesis.