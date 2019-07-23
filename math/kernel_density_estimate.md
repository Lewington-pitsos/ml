# Kernel Density Estimates

## Histogram

Let's start with histograms. Intuitively what a histogram does is tell us what percentage/proportion of the data falls between a given interval, for like eight, sequential intervals. 

Mathematically (ugh), we can define the Histogram function:

`N` = number of data points in the whole set
`n(x)` = number of data points in the same "bin"/segment/interval as `x`
`w` = width of bin (e.g. a bin might be 2.5 - 3.6, so it's width is 1.1)

    f(x) = (1/N) * (n(x) / w)  

so it returns a value which roughly means "the proportion of the whole dataset that's in the same bin as `x`". It's assumed we know what the bin width for `x`'s bin is already.

## Kernel Density Estimator

KDA is a similar function to the histogram function.