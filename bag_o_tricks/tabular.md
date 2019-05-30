- Replace null values with the existing *average* or *median* for that column. That way these values aren't allowed to fuck with the statistics of the column *and* they probably also better approximate the actual value that should be there.

- Split training into 10 folds and validate 10 times, each with a different validation set

- Column ORDER apparently can have a (small) impact on learning for some fucked reason.


