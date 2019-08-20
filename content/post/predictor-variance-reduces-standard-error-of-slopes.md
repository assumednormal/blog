---
title: "Predictor Variance Reduces Standard Error of Slopes"
date: 2019-08-19
tags: []
mathjax: true
disableComments: false
---

A common solution to moderate degradation in marketing or risk models is to re-fit the same model on newer vintages. This approach has at least two benefits.

1. Modelers can open the original model fitting code, drop in the new data set, and be done with the re-fit in a few hours.
2. The original model would have gone through a comprehensive review of methodology, performance, and implementation. Having followed the same methodology, all that would be left to review is performance and implementation.

Re-fitting an existing model also makes sense especially when there have been no notable changes in targeting or credit policy. When the newer vintages contain a more homogeneous population than the population used for the original model development, seemingly strong predictors can drop out of the model. This should be expected because variance in the predictors reduces variance of coefficient estimates.

As a simple example, suppose you fit a simple linear regression: $\hat{y}=\hat{\beta}\_0+\hat{\beta}\_1 x$. We can calculate the standard error of $\hat{\beta}\_1$, with $\hat{\varepsilon}\_i$ being the residual for the $i^{\textrm{th}}$ sample.

$$s\_{\hat{\beta}_1} = \sqrt{\frac{\frac{1}{n-2}\sum\_{i=1}^{n}\hat{\varepsilon}\_i^2}{\sum\_{i=1}^{n}\left(x\_i-\bar{x}\right)^2}}$$

As the variance in $x$ grows, the standard error of $\hat{\beta}\_1$ decreases.