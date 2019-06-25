# Timeseries stuff

Basically the presence of a timeseries in your dataset means you can do a few fancy things.

## Lags

Ok, so let's say you want to predict the sales in woolworths for january 2020. A good thing to look at might be the sales for woolworths in january 2019, right? Another good thing to look at might be the sales for december 2019. 

More generally: there's a pretty intuitive principle which states "if you want to predict something about an event in the future, look at instances of that event in the past". 

This is intuitive to humans though, not your LassoCV or whatever. So even if you have a nice timeseires going on, your model won't know to compare values in the present with values in the past. 

The solution is to create `lag features` explicit additional features that are just carbon copies of values from existing features in some (consistent) time in the past. So, for instance, with the woolworths example, you might introduce a `last_month_sales` column, which is just the `sales` column, copied up one month. 

### Fun Lag things:

- Try making lags of your actual target in your test set

## Rolling Window Stats

So lags are good, but what about lagged *statictis*. Like, last month's sales are probably indicative, but noisy. A more consistent metric might be the average of the last 3 month's sales. Aggregated statistics of lags like this are called `rolling window statictis`, presumably because, when gathering these statistics, you're looking at  small, tempotally sequential subsections of the dataset, kind of like if you held up a frame in front of you and only gathered stats about what you could see through the frame. 

### Expanding Windows

Same deal except you stretch the frame instead of moving it. I.e. an expanding window mean for, say `sales` is the mean of *all* prior `sales` values. Again this might be useful.