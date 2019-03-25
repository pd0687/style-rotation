For the style rotation project, I'm just going to stick to logistic regression. 

The factors that I've settled on for the style rotation project, consistent with the article in Journal of PF Management, are:
— inflation (CPI)
— productivity (CFNAI)
— coincident indicator (money supply)
— Y_t (more on this below)
— 3M TBill yield
— term structure (20Y minus 3M)

Further consistent with the article, I'll lag these regressors by one period.  
The strategy that I'll model will be small cap (SP600) vs. large cap (SP500).

My target AKA dependent variable will be the monthly return spread.  For example, I'll take the February versus January return for small, map that to "Feb," and then subtract the same for large.  I'll then coerce this to Boolean (i.e., 1 if positive, 0 otherwise).

Notice how one of my factors is Y_t.  So for example, in the small versus large strategy, I'll use the monthly spread between small and large for Feb minus Jan as a predictor for the target, which is the spread for March minus Feb.  Given that I lagged the regressors, all are as of time t, and the target is as of time t+1.

Sensitivity analysis includes:
— varying logistic regression's hyperparameter C, which is an inverse regularizarion term: the higher the C, the lower the regularizarion
— length of training period (the article uses six months)
— regressors (specifically, do I also include Y_t-1 and/or do I include/exclude other macro regressors)
— allocation/switching threshold (should the bar be 50% or something else, should I include a buffer to reduce noisy trading, etc.)

I'm thinking of what preprocessing I need to perform on the data.  Should I standardize the data?  I'm just trying to envision how that should be done.  In particular, do I just standardize on each rolling training period (n = 6), or do I standardize each regressor over the entire period (n = 12 * # of years)?

While the target is binary, the target lag, which is an independent variable, will be continuous (i.e., the pure spread, not coerced to Boolean).  I don't see why this should be a problem.  Or is it?

Should the same value of C should be used for the entire period, or can it vary
with each rolling six-month training period?

This project plays well to a confusion matrix.  For asset management purposes, "type 1" would be to switch allocation if you shouldn't (false positive), whereas "type 2" would be to not switch allocation when you should (false negative).  These are considerations for improving precision versus recall.

I'll use as baselines each S&P index.  That is, I'll use 100% small cap and
100% large cap as baselines.  And the overall objective will be to improve
precision, recall, and accuracy.