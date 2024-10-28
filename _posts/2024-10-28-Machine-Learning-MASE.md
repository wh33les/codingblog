---
layout: post
title: Machine learning&#58; MASE
date: 2024-10-28 #13:00:00 -06:00
---
I recently completed a forecast project for the 2024 U.S. presidential election that used a double exponential smoothing algorithm [(you can see the project here, along with some info about double exponential smoothing)](https://wh33les.github.io/2024ElectionForecast).  In developing the model, I used a grid search with the metric MASE, which stands for **mean absolute scaled error**.

For the grid search, I used cross-validation for time series, with the training data as selected polling data from the day Biden dropped out of the race to the most recent poll, with five splits, and with holdout sets of seven days each.  The hyperparameters $\alpha$ and $\beta$ varied in each train-train set, from 0.0 to 1.0, in 0.01 increments.  So for each cv split I tested 100 combinations of $\alpha$ and $\beta$, then took the minimum MASE from the 500 models, and I did this for each of the two candidates.

So what is MASE?  In short, the MASE is the mean absolute error of the model divided by the mean absolute error of the na&iuml;ve forecast.  Mean absolute error is the average of the distances between the predicted values and the observed values at each time step.  Since MASE is a ratio of the error of the model's forecast to the error of the baseline (na&iuml;ve) forecast, a MASE value greater than $1$ means the model is worse than the baseline, and a MASE value less than $1$ means the model is better.   

### Na&iuml;ve forecast

The na&iuml;ve forecast, when the time series is not seasonal, simply predicts the current value of the time series to be the last observed value, or

$$\hat y_t = y_{t-1}$$

(the polling data is not seasonal).  It is seen as a pretty reliable baseline, as time series data is often hard to predict.  For example, with polling data, anything in the news could affect poll numbers.  It is not clear from one day to the next how the polls will behave.  (Although there may be a small trend, and this is why I used double exponential smoothing for my forecast.)

### Formula for MASE

Let $\hat y_t$ be the model's prediction of the time series at time $t$ and $y_t$ the observed value at time $t$ (in general, $\hat y$ always denotes a model's predicted value in machine learning while $y$ denotes the observed value in the test data).  If the holdout data has observations $y_1,\dots y_N$, then MASE is given by 

$$\frac{\sum_{t=1}^N|\hat y_t-y_t|}{\sum_{t=1}^N|y_{t-1}-y_t|}.$$

### Advantages of MASE

(1) MASE is independent of the scale of the forecast.  It is a ratio of the errors, so the numerator and denominator have the same scale and it cancels out.

(2) MASE is symmetric in the sense that positive and negative errors are treated the same, via the absolute values in the formula.

(3) MASE behaves well for small observation values.  In an alternative method, MAPE [(mean absolute percentage error)](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error), the formula uses observed values in the denominator, and so when observed values are small MAPE gets inflated.  This does not happen with MASE.



