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
<br><br>
$$
  Y_{i}\sim N(X_{i}^{T}\beta ,\sigma ^{2})
$$
<br><br>
and paremeters are estimated as 
<br><br>
$$
  \hat{\beta} = (X^{T}X)^{-1}X^{T}Y
$$
<br><br>
$$
  \hat{\sigma}^{2} = Y^{T}(I-H)Y/rank(I-H)
$$
<br><br>
where 
$$
  H=(X^{T}X)^{-1}X^{T} 
$$
is a projection matrix. Also, sampling distribution of
$$
  \hat{\beta} 
$$
is:
<br><br>
$$
  \hat{\beta}\sim N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1}) 
$$
<br><br><br>In **Bayesian** approach, posterior distribution
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
<br><br><br>
For the first prior, posterior distribution is derived as: 
<br>
$$
  P(\beta|Y,X) 
$$
<br><br>
$$
  = \frac{P(Y|\beta,X)P(X|\beta)P(\beta)}{P(Y,X)}
$$
<br><br>
$$
  \propto P(Y|\beta,X)P(\beta)
$$  
<br> 
$$
  = L(\beta)\prod_{k=0}^{p}P(\beta_{k}) 
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
where second equality holds only if 
$$
  Cov(\beta_{i},\beta_{j})=0 
$$
for all i,j.
<br><br>
Thus, proportional of 
$$
  \beta|Y,X
$$
follows
<br><br>
$$
  Prop(\beta|Y,X) \propto N((X^{T}X)^{-1}X^{T}Y,\sigma^{2}(X^{T}X)^{-1})
$$
, 
<br><br>
where 
$$
  Prop(\beta|Y,X)
$$
refers to the proportional of 
$$
  \beta|Y,X
$$
. This is the same as the sampling distribution of
$$
  \hat{\beta}
$$
. The result is not surprising, since prior distribution is extremely noninformative.
<br><br><br>Now, let's take
$$
  N(0,\lambda)
$$
as prior. After the same process
<br><br>
$$
  P(\beta|Y,X) \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})\times exp(-\frac{\beta^{T}\beta}{2\lambda})
$$
,<br><br>
 whose logarithm takes the following form:
<br><br>
$$ 
  log(P(\beta|Y,X)) \propto -\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}}-\frac{\beta^{T}\beta}{2\lambda}
$$
<br><br>
This is the similar to the loss function of ridge regression, with 
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
<br><br>
$$
  Prop(\beta|Y,X) \sim N((X^{T}X+\frac{\sigma^{2}}{\lambda}I)^{-1}X^{T}Y,(\sigma^{-2}(X^{T}X)+\lambda^{-1}I)^{-1})
$$
<br><br>
, which is the same form as sampling distribution of
$$
  \hat{\beta}
$$
in ridge regression.
<br><br><br>As the third prior, let's take
$$
  Laplace(0,\lambda)
$$
. Similarly,
<br><br>
$$
  P(\beta|Y,X) \propto exp(-\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}})\times exp(-\frac{\sum_{k=0}^{p}\left | \beta_{k} \right |}{2\lambda})
$$
<br><br>
and after taking logarithm, following is derived:
<br><br>
$$ 
  P_{prop}(\beta|Y,X) \propto -\frac{(Y-X\beta)^{T}(Y-X\beta)} {2\sigma^{2}}-\frac{\sum_{k=0}^{p}\left | \beta_{k} \right |}{2\lambda}
$$
,<br><br>
which is similar to the loss function of lasso regression, with 
$$
  \lambda
$$
controlling the degree of L1 regularization. 
<br><br>
In this case, there is no closed form expression for posterior distribution, just like lasso regression.
Threrfore, to sample
$$
  \beta
$$
from the posterior distribution, sampling methods such as importance sampling and rejection sampling should be employed.
<br><br><br>
The following table is the comparison of parameters using different models, using CO2 data in R:
<br>  
- model1: model with no regularization (constant prior)
- model2~4: with regularization parameter 
$$
  \lambda
$$  
  =0.1, 10, 100000 (normal prior)

| | model1 | model2 | model3 | model4 |
| :--- | :---: | :---: | :---: | :---: |
| Intercept | 29.26 | 1.25 | 22.70 | 29.26 |
| Mississippi | -12.66 | -0.07 | -8.95 | -12.66 |
| Chilled | -6.86 | 0.24 | -4.05 | -6.86 |
| Conc | 0.02 | 0.05 | 0.02 | 0.02 |

Compared to model1, there is significant effect of regularization in model2, making parameters significantly differ from ones from model1. On the other hand, there is little effect of regularization in model4, where 
$$
  \lambda
$$
has very large value.
<br><br><br>Then, how this framework works when it comes to predicting new data
$$
  Y_{N+1}
$$
? In this post, Monte Carlo method will be used to handle this problem. In other words, 
<br><br>
$$
  \hat{Y}_{N+1} \approx \frac{1}{n}\sum_{i=1}^{n}\tilde{Y}^{(i)}_{N+1} 
$$
<br><br>
where
$$
  \tilde{Y}^{(i)}_{N+1} \sim N(Y_{N+1}|\mathbf{X},X_{N+1},\mathbf{\beta}^{(i)})
$$
<br><br>
Let 83th and 84th data be test data and all the other remainings be train data. 
Followings are prediction result of model1
![Prediction]({{ site.urlimg }}/Bayesian-Linear-Regression/Prediction.png "Prediction")
<br>
where blue lines indicate true values. It seems that prediction result is not that good in this case.  
<br><br><br>**Summary**
<br>In this post, followings were covered: 
- Bayesian linear regression using different prior distributions
- Prediction using Bayesian linear regression model
