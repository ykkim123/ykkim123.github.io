---
layout: post
title: "Bayesian Linear Regression"
date: "2020-02-24"
description: "This post is about linear regression from Bayesian approach; specifically, it's about linear regression from empirical Bayesian approach using different prior distributions."
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

Linear regression is one of the most frequently used statistical methods because of its usefulness in interpretation. 
<br>In **frequentist**'s view, linear regression is formulated as:
<br>
$$
  Y_{i}\sim N(X_{i}^{T}\beta ,\sigma ^{2})
$$
<br>
and paremeters are estimated as 
<br>
$$
  \hat{\beta} = (X^{T}X)^{-1}X^{T}Y
$$
<br>
$$
  \hat{\sigma}^{2} = Y^{T}(I-H)Y/rank(I-H)
$$
<br>
where 
$$
  H=(X^{T}X)^{-1}X^{T} 
$$
is a projection matrix. Also, the sampling distribution of
$$
  \hat{\beta} 
$$
is:
<br>
$$
  \hat{\beta}\sim N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1}) 
$$
<br><br>In **Bayesian** approach, posterior distribution
$$
  P(\beta|Y,X)
$$
is used to make inference about 
$$
  \beta
$$
. To calculate posterior distribution, there are several prior distributions commonly used: 
<br><br>
$$
  \cdot P(\beta_{k})\propto 1
$$
<br>
$$
  \cdot \beta_{k}\propto N(0,\lambda)
$$
<br>
$$
  \cdot \beta_{k}\propto Laplace(0,\lambda)
$$
<br><br>
For the first prior, posterior distribution is derived as: 
$$
  P(\beta|Y,X) 
$$
<br>
$$
  = \frac{P(Y|\beta,X)P(X|\beta)P(\beta)}{P(Y,X)}
$$
<br> 
$$
  \propto P(Y|\beta,X)P(\beta)
$$  
<br> 
$$
  = L(\beta)\prod_{k=0}^{p}P(\beta_{k}) 
$$  
<br>
$$
  (assume Cov(\beta_{i},\beta_{j})=0 for all i,j)
$$
<br>
$$
  = L(\beta)
$$
<br>
$$
  \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})
$$
<br><br>
Thus, proportionanl of 
$$
  \beta|Y,X
$$
follows
<br>
$$
  \beta|Y,X \propto N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1})
$$
<br><br>
which is the same as sampling distribution of
$$
  \hat{\beta}
$$
. The result is not surprising, since prior distribution is extremely noninformative. 
<br><br>Now, let's take
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
<br>As the third prior distribution, let's take
$$
  \Laplace(0,\lambda)
$$
as prior distribution. Similarly, 
$$
  P(\beta|Y,X) \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})\times exp(-\frac{\sum_{k=0}^{p}\left | \beta_{k} \right |}{2\lambda})
$$
<br>
After taking logarithm, following is derived:
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
In this case, there is no closed form expression for a posterior distribution, just like lasso regression.
To sample
$$
  \beta
$$
from the posterior distribution, sampling methods(e.g. Metropolis-Hastings algorithm, importance sampling) should be employed.
<br><br>Then, let's check if regularization actually works. In this example, CO2 data from R was used. 
<br>Models compared are: 
<br>
$$  
  - model with no regularization(constant prior distribution, model1)
$$
<br>
$$
  - model with regularization parameter \lambda =0.1, 10, 100000 (normal prior distribution, model2,3,4 respectively)
$$
<br><br>
The following table shows its result: 
<br>
Compared to model1, there is significant effect of regularization in model2, 
making parameters significantly differ from ones from model with no regularization. 
On the other hand, there is little effect of regularization in model4.
<br><br>Then, how does posterior distribution works when predicting new data 
$$
  Y_{N+1}
$$
? In this post, Monte Carlo method will be used to handle this problem. In other words, 
<br>
$$
  \hat{Y}_{N+1} \approx \frac{1}{n}\sum_{i=1}^{n}\tilde{Y}^{(i)}_{N+1}
$$
where
<br>
$$
  \tilde{Y}^{(i)}_{N+1} \sim N(Y_{N+1}|\mathbf{X},X_{N+1},\mathbf{\beta}^{(i)})
$$
<br><br>
Using CO2 data continuously, let 83th and 84th data be test data and all other remaining be train data. 
Followings are prediction result of different models from above example.










