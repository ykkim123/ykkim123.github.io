---
layout: post
title: "Introduction to Bayesian Statistics"
date: "2020-02-24"
description: "This post is about linear regression from Bayesian approach. Specifically, it deals with standard linear regression logistic regression using posterior distribution"
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Bayesian
  - regression
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
---

Linear regression is one of the most frequently used statistical methods because of its usefulness to interpret. 
In frequentist's view, linear regression is formulated as:
<br>
$$
  Y_{i}\sim N(X_{i}^{T}\beta ,\sigma ^{2})
$$
<br>
and paremeter
$$
  \beta
$$
and
$$
  \sigma^{2}
$$
are estimated as 
<br>
$$
  \hat{\beta} = (X^{T}X)^{-1}X^{T}Y
$$
<br>
  \hat{\sigma}^{2} = Y^{T}(I-H)Y/rank(I-H) (H=(X^{T}X)^{-1}X^{T} is a projection matrix)
$$
<br>
respectively, and 
$$
  \hat{\beta}\sim N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1}) 
$$
<br>In Bayesian approach, posterior distribution
$$
  P(\beta|Y,X)
$$
is used to make inference about 
$$
  \beta
$$
. To calculate posterior distribution, there are several prior distributions commonly used: 
<br>
$$
  - p(\beta_{k})\propto 1
  - \beta_{k}\propto N(0,\lambda)
  - \beta_{k}\propto Laplace(0,\lambda)
$$
For the first prior distribution, posterior distribution is derived as: 
$$
  P(\beta|Y,X) = \frac{P(Y,X,\beta)}{P(Y,X)}
  <br> = \frac{P(Y|\beta,X)P(X|\beta)P(\beta)}{P(Y,X)}
  <br> \propto P(Y|\beta,X)P(\beta)
  <br> = L(\beta)\prod_{k=0}^{p}P(\beta_{k}) (assume Cov(\beta_{i},\beta_{j})=0 for all i,j)
  <br> = L(\beta)
  <br> \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})
$$
<br>
Thus, proportional of 
$$
  \beta|Y,X
$$
  follows 
$$
  \beta|Y,X \propto N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1})
$$
, which is the same as sampling distribution of
$$
  \hat{\beta}
$$
.
<br>The result is not surprising, since prior distribution is extremely noninformative. 
<br>Now, let's take
$$
  N(0,\lambda)
$$
as prior distribution.
<br>After the same process, 
$$
  P(\beta|Y,X) \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})\times exp(-\frac{\beta^{T}\beta}{2\lambda})
$$
<br>
After taking logarithm, it takes the following form:
<br>
$$ 
  -\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}}-\frac{\beta^{T}\beta}{2\lambda}
$$
<br>, which is the similar form as the loss function of ridge regression, with 
$$
  \lambda
$$
controlling the degree of L2 regularization. 
<br><br>
Thus, proportional of 
$$
  \beta|Y,X
$$
  follows
$$
  (X^{T}X+\frac{\sigma^{2}}{\lambda}I)^{-1}X^{T}Y,(\sigma^{-2}(X^{T}X)+\lambda^{-1}I)^{-1}
$$
, which is the same as sampling distribution of
$$
  \hat{\beta}
$$
in ridge regression.
<br>Now, let's take
$$
  \Laplace(0,\lambda)
$$
as prior distribution.
<br>After the same process, 
$$
  P(\beta|Y,X) \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})\times exp(-\frac{\sum_{k=0}^{p}\left | \beta_{k} \right |}{2\lambda})
$$
<br>
After taking logarithm, it takes the following form:
<br>
$$ 
  -\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}}-\frac{\sum_{k=0}^{p}\left | \beta_{k} \right |}{2\lambda}
$$
<br>, which is the similar form as the loss function of lasso regression, with 
$$
  \lambda
$$
controlling the degree of L1 regularization. 
<br><br>
Like lasso regression, there is no closed form expression for a posterior distribution.
To sample
$$
  \beta
$$
from above conditional distribution, sampling methods(e.g. Metropolis-Hastings algorithm, 
importance sampling) should be employed.
<br>Then, how does posterior distribution works when predicting new data 
$$
  Y_{N+1}
$$
? In this post, Monte Carlo method will be used to handle this problem. In other words, 
$$
  \hat{Y}_{N+1} \approx \frac{1}{n}\sum_{i=1}^{n}\tilde{Y}^{(i)}_{N+1}
$$
<br>where
$$
  \tilde{Y}^{(i)}_{N+1} \sim N(Y_{N+1}|\mathbf{X},X_{N+1},\mathbf{\beta}^{(i)})
$$
<br><br>In previous example, *mle* of 
$$
  \sigma^{2}
$$
was used to find posterior distribution of 
$$
  \beta
$$
(empirical Bayesian approach). But it's conceptually more appropriate to find joint posterior distribution of
$$
  \beta, \sigma^{2}
$$
. In this case, there are also several prior distributions commonly used: 
$$
  - P(\beta,\sigma^{2}) \propto \frac{1}{\sigma^{2}}
$$
$$
  - P(\beta|\sigma^{2})P(\sigma^{2}) = N(\mu,\sigma^{2}\sum) \times\ InvGamma(\nu,\rho)
$$
$$
  - P(\beta|\sigma^{2},\rho)P(\sigma^{2}|\rho)P(\rho) = N(\mu,g\sigma^{2}(X^{T}X)^{-1}) \times InvGamma(\sigma^{2};a,b) \times InvGamma(g;c,d) (Zellner's g-prior)
$$








