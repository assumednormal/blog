---
title: "Finite Population Correction"
date: 2019-08-01
tags: []
draft: true
mathjax: true
disableComments: false
---

When sampling with replacement from finite populations, we have to change how we think about sampling from the larger population. Now what we measure on each sampled individual is fixed: a person's height and weight, a family's annual income, etc. What is considered random is whether or not each individual is selected for inclusion in the sample.

Suppose there are $N$ individuals in a population. We take a sample of $n$ individuals without replacement and measure their heights, $h\_i$, $i=1,\ldots,n$. Let $Z\_i$ be the (random) indicator for the $i^{\textrm{th}}$ individual being included in the sample.

We can write down the population mean and variance.

$$
\begin{align}
\mu &= \frac{1}{N}\sum\_{i=1}^{N}h\_i\newline
&\phantom{=} \newline
\sigma^2 &= \frac{1}{N}\sum\_{i=1}^{N}\left(h\_i-\mu\right)^2\newline
&= \frac{1}{N}\sum\_{i=1}^{N}\left(h\_i^2-2\mu h\_i+\mu^2\right)\newline
&= \frac{1}{N}\sum\_{i=1}^{N}h\_i^2-\mu^2\newline
&= \frac{1}{N}\sum\_{i=1}^{N}h\_i^2-\frac{1}{N^2}\sum\_{i=1}^{N}\sum\_{j=1}^{N}h\_ih\_j\newline
\end{align}
$$

We can write down the sample mean as well.

$$\bar{H}=\frac{1}{n}\sum\_{i=1}^{N}h\_iZ\_i$$

The sample variance is less obvious.

The $Z\_i$ are apparently Bernoulli distributed with parameter $p=n/N$. The sample size is fixed at $n$ though, so there must be a negative correlation between $Z\_i$ and $Z\_j$, for $i, j\in\left[1,\ldots,n\right]$ and $i\neq j$.

$$
\begin{align}
\mathbb{V}\left[\bar{H}\right] &= \frac{1}{n^2}\mathbb{V}\left[\sum\_{i=1}^{N}h\_iZ\_i\right]\newline
&= \frac{1}{n^2}\left(\mathbb{E}\left[\left(\sum\_{i=1}^{N}h\_iZ\_i\right)^2\right]-\mathbb{E}^2\left[\sum\_{i=1}^{N}h\_iZ\_i\right]\right)\newline
&= \frac{1}{n^2}\sum\_{i=1}^{N}\sum\_{j=1}^{N}h\_ih\_j\left(\mathbb{E}\left[Z\_iZ\_j\right]-\frac{n^2}{N^2}\right)\newline
\end{align}
$$

Notice that $Z\_iZ\_j=1$ when both individuals $i$ and $j$ are in the sample. Otherwise $Z\_iZ\_j=0$. There are $\binom{N}{n}$ possible samples we could have selected. When $i\neq j$, $\binom{N-2}{n-2}$ samples contain both individuals $i$ and $j$.

$$
\begin{align}
\mathbb{V}\left[\bar{H}\right] &= \frac{1}{n^2}\left(\sum\_{i=1}^{N}\sum\_{j\neq i}^{N}h\_ih\_j\left(\frac{n\left(n-1\right)}{N\left(N-1\right)}-\frac{n^2}{N^2}\right)+\sum\_{i=1}^{N}h\_i^2\frac{n\left(N-n\right)}{N^2}\right)\newline
&= \frac{N-n}{N-1}\frac{1}{n}\left(\sum\_{i=1}^{N}\sum\_{j\neq i}^{N}h\_ih\_j\frac{-1}{N^2}+\sum\_{i=1}^{N}h\_i^2\frac{N-1}{N^2}\right)\newline
&= \frac{N-n}{N-1}\frac{1}{n}\left(\frac{-1}{N^2}\sum\_{i=1}^{N}\sum\_{j=1}^{N}h\_ih\_j+\frac{1}{N}\sum\_{i=1}^{N}h\_i^2\right)\newline
&= \underbrace{\frac{N-n}{N-1}}\_{\textrm{FPC}}\frac{\sigma^2}{n}\\
\end{align}
$$