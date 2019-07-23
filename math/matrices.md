## Matrix Vector Multiplication

If a matrix `X` has the dimensions `m` * `n` (e.g., 2 * 3), you can *only* multiply it by a vector `v` of length `n` (i.e. has as many numbers in it as `X` has *columns*, in this case 3).

    `X` = [[2, 4, 5],
        [1, 3, 5]]

    `v` = [3, 1, 1]


The result is a vector `v2` of length `m`. It's first value will be the sum of each value in `v` multiplied against the corresponding value of `M`'s first row. The second value will be the sum of each value in `v` multiplied against the corresponding value of `M`'s second row... and so on.

In this case

    `v2` = [
        3 * 2 + 1 * 4 + 1 * 5,
        3 * 1 + 1 * 3 + 1 * 5 
    ]    

    `v2` = [15, 11]