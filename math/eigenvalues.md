# eigenvalues

An *eigenvalue* is a special value that exists/is defined only for certain matrices. 

For a matrix `M` to have an eigenvalue, there must be a vector `v` and a scalar `s` out there such that `M` * `v` = `v` * `s` *and `v` contains no 0 values. The scalar is the eigenvalue. Incedentally `v` is called an eigenvector.

Now, for starters we know that a matrix `M` multiplied against a vector `v` is only possible if `M` has as many columns as `v` has values. We also know that the resulting vector `u` will have the same number of values as `M` has rows. So if a matrix `M` has a different number of columns and rows, the result of multiplying it against any vector `v` will be a vector `u` whose length is not the same as the length of `v`, so `v` and `u` can never be equivalent.

So, right out the gate, we can see that no matrix can have an eigenvalue unless it has the same number of rows as columns.

We can also see that if `v` *was* allowed to contain 0's, then literally *any* value at all would be an eigenvalue for any square matrix `M` (since `v` could just contain only 0's, so `M` * `v` would just be a vector of 0's, as would `v` * `s`, for any `s`). 

A lot of matrices don't have eigenvalues though, even if they are square.

Consider the random matrix `Q`:

    1, 2, 4
    2, 3, 2
    4, 5, 7

Does `Q` have an eigenvalue? It's not instantly apparent, but it doesn't look like it, right? Matrices with eigenvalues *do* exist though. Consider the **identity matrix** `I`:

    1, 0 ,0
    0, 1 ,0
    0, 0 ,1

because `Q` * any vector `w` is just `w`, 1 is an eigenvalue for `Q`, since `M` * `v` = `v` * 1.

## What kinds of matrices will have eigenvalues?

Well, we know that if `v` is a vector, and  `I` is an identity matrix with the same length as `v`, and that `v` * `s` = `x`, then `v` * (`I` * `s`) = `x`.

This is because when we multiply a scalar `s` by a vector `v`, we multiply every element in `v` by `s`. And when we multiply `v` by another vector `u`, we multiply each element in `v` by the corresponding element in `u`. It follows that if all the values in `u` are `s`, then `v` * `u` = `v` * `s`, since the operations end up being identical. 

In addition we know that if we multiply `s` by an identity matrix `I`, we end up with a matrix `S` whose values are all 0's except for the diagonal, whose values are all `s`. We know that if er multiply any vector `g` against `S`, we will get the same as multiplying `g` against `s`, since `g` * `S` just ends up multiplying `s` against all of `g`'s values.

so `g` * `s` = `g` * (`I` * `s`), where `I` is an identity matrix with the same length as `g`.

It follows that  if `M` * `v` = `v` * `s`, then `M` * `v` = `v` * (`I` * `s`), or or rather: `M` * `v` = (`I` * `s`) * `v`. 

We also know that if some matrix `R` * some vector `w` = some other matrix `S` * `w`, then (`R` - `S`) * `w` = 0.

It follows that, since `M` * `v` = (`I` * `s`) * `v`, then (`M` - (`I` * `s`)) `v` = 0

so if a matrix has a eigenvalue, if you subtract the identity matrix times the eigenvalue from the matrix, and multiply the result by the eigen*vector*, you get 0.


In fact only a very particular kind of matrix will have an eigenvalue. In fact, all values are eigenvalues 