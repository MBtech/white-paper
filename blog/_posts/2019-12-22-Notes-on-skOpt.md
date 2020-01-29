---
layout: post
title: "Notes on skOpt"
categories:
  - notes
tags:
  - notes
---
There are some notes regarding the implementation of skOpt

- the surrogate models are called base estimators
- Is the base estimator is ET Regression, RF regressor or Gradient Boosting Quantile Regressor then it doesn't have a gradient
- If a base estimator has gradients and `acq_optimizer` is set to `auto` then `lbfgs` is used for acquisition optimization otherwise `sampling` is used.




**References:**
