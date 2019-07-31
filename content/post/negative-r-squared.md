---
title: "Negative R-Squared"
date: 2019-07-30
tags: []
mathjax: true
disableComments: false
---

The coefficient of determination is typically written as $\textrm{R}^2=1-\textrm{SS}\_{\textrm{res}}/\textrm{SS}\_{\textrm{tot}}$. But there are a few assumptions behind the simple formula that lead to nonsensical results when violated.

Suppose you fit a simple linear regression on some data set. If the independent and dependent variables have non-zero means, the intercept will necessarily be non-zero. If you force the intercept to zero, the $\textrm{R}^2$ of the fitted line may be less than zero.

```r
library(dplyr)

set.seed(7)

d <- data.frame(x = rnorm(100)) %>%
  mutate(y = 10 + x + rnorm(n()))

rsq <- function(y, y_hat) {
  ss_tot <- sum((y - mean(y))^2)
  ss_res <- sum((y - y_hat)^2)
  return(1 - ss_res / ss_tot)
}

full_model <- lm(y ~ x, data = d)
rsq(d$y, fitted(full_model))
# [1] 0.5100709

no_intercept_model <- lm(y ~ -1 + x, data = d)
rsq(d$y, fitted(no_intercept_model))
# [1] -54.70656
```

There are two violated assumptions in this example: (i) the residuals have non-zero mean and (ii) the residuals are correlated with the independent variable, $x$.

```r
round(cor(d$x, resid(full_model)), 2)
# [1] 0
round(cor(d$x, resid(no_intercept_model)), 2)
# [1] -0.84

round(mean(resid(full_model), 2))
# [1] 0
round(mean(resid(no_intercept_model), 2))
# [1] 10
```