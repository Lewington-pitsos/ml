# Missing value classes

Not all missing values should be treated the same. For instance, if we have a political survey which contains a bunch of mundane questions, and then one super weird question like "how many times do you have sex per week" mission values for that question should be treated differently from missing values in normal questions. In the latter case, mission responses are probably more or less random (participant had to run off for example, or didn't understand the question). But in the former case, missing values likely indicate a refusal of the participant to answer, which in turn might tell us something about the participant. 

More to the point: while something like mean imputation might work in the latter case, using it for the latter case might destroy valuable data. 

## The Classes

To help us deal with this, statisticians have come up with 3 broad classes of missing valus

### MCAR: Missing Completely at Random

These value are mission literally owing to pure chaos. There is no relationship between missing values and any other values in your dataset (observed or missing), *or* in fact, any other data out ther in the world that you're aware of. 

A pure example is literally a random number generator removed values at random. A more practical example might be that some of the papers upon which some results were written got lost.

MCAR is kind of easy to deal with, you can use something like mean or multiple imputation and you should get a decent estimate.

### MAR: Missing At Random

These values are missing somewhat randomly, but the propensity for a value to be missing is related to some other observed variable. E.g. if we're surveying lawn ornaments, and one of the possible ornaments is "a huge screen blocking the lawn", clearly we would expect other possible orniments like "happy gnome" to have missing values more often when "a huge screen blocking the lawn" is marked as present. 

The idea is that if you *control* for the influencing variable, we're back to MCAR. For instance if you only look at the subset of the above data where there are no screens present, "happy gnome" will probably be MCAR.


### MNAR: Missing Not At Random

There is a bit more controversy about what this means exactly, but largely it seems to refer to values whose propensity to be missing does not appear related to any recorded data, but also doesn't appear to be random. For instance, in the previous example you know that people who live at high altitudes tend to have high fences, which could lead to missing values under, say "happy gnome", but sadly the data doesn't record house altitude. 


## SO what?

Well basically these values warrant different treatment. For instance, *multiple imputation* works well for MCAR values, but nor MNAR values, since the underlying distribution of the missing variables isn't random, so your multiple random plausible imputation strategies will yield an approximation that isn't in line with reality. Same with MAR, although with MAR you can use your observed data to weight the imputation procedures, and so rescue the  