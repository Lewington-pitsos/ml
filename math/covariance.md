# covariance 

Covariance is a measure of the relationship between two variables, X and Y. X and Y have higher covariance the more that, when they change (vary), they change in the same direction. So, height and weight in people have high covariance (where hight increases so does weight). Height and number of teeth have no covariance, since when one changes the other usually stays the same. Tennis games lost and tennis skill have a negative covariance, since when one increases the other tends to decrease and 
vise versa. 

Covariance doesn't measure the strength of the relation between two variables, just whether it is positive, negative or non existent.

## formula

where we have two variables `X` and `Y`, whose samples are `x` and `y` respectively,

    cov = sum((x - mean(X)) * (y - mean(Y))) for all samples / number of samples

The intuition behind this is that you'll get a positive covariance if `x`'s tend to be above the average of `X` while `y`'s are above the average of `Y`, similar if both tend to be below the average at the same points. In either case, the sum will increase. When `x` is below the average of `X` while `y` is above the average of `Y` or vise versa, the sum of covariance will decrease (since positive * negative = negative). 

If the covariance is around 0, it means that the two variables being above or below their averages at the same time happens about the same amount, or to the same extent as the two variables being above or below their averages at different times, meaning that they change more or less independently of one another.

# Covariance matrix ????

A covariance matrix `C` can be calculated from any matrix `M`. Is it simply `M` * the transposition of `M`.

## uses

Conceptually covariance is a little bit like correlation. It describes the relationship between two variables. Unlike correlation, which measures the *degree* of linear similarity between two variables, covariance only indicates whether there *is* any linear relationship between two variables, and whether the variable's values converge or diverge.

Covariance does not tell us about the *strength* of a linear relationship.