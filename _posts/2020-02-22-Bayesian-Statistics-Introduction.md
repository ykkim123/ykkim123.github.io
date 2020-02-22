---
layout: post
title: "Introduction to Bayesian Statistics"
date: "2020-02-20"
description: "Brief introduction to Bayesian statistics"
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Bayesian_statistics
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
---

Bayesian statistics is certainly one of the hottest area in modern statistics. 
<br>To illustrate difference between frequentist and bayesian view, there is an simple example: 
<br>Person A toss a coin, where
$$
  Y_{i}
$$
is a random variable that represents ith result of coin toss among N observations and follows a Bernoulli distribution, i.e.,
$$
  Y_{i} ~ Ber(p)
$$
that takes either 1(head) or 0(tail). Also, assume further that a is the number of heads.  
<br>in **frequentist view**, parameter 
$$
  p
$$
can be estimated as mle(maximum likelihood estimator), 
$$
  \hat{\theta} = \frac{\sum_{i=1}^{N}Y_{i}}{N},
$$
which maximizes likelihood function
$$
  P(\theta|Y)
$$
<br>
{:.text-center img}
![Toy example]({{ site.urlimg }}/media/apple-icon.png "Toy example")

Unlike frequentist view, in **Bayesian**, since parameter
$$
  p
$$
is a random variable, the following formula can be derived:
<br>
$$
  P(\theta|Y)=\frac{P(Y|\theta)P(\theta)}{P(Y)} \propto P(Y|\theta)P(\theta)
$$
since denominator is predetermined(fixed).
<br>
  From the above formula, followings are called as:
$$
  - P(Y|\theta) : likelihood
  - P(\theta) : prior distribution
  - P(\theta|Y) : posterior distribution
$$
<br>The posterior distribution can be viewed as weighted average of likelihood function, with prior distribution functioning as weight.
<br>Plus, if the posterior distribution is in the same probability distribution family as the prior probability distribution, 
the prior and posterior are called conjugate distributions, and the prior is called a conjugate prior for the likelihood function. 
<br>After some algebra, posterior distribution is:
$$
  P(\theta|Y) ~ Beta(a+\alpha,N-a+\beta) 
(Here, 
$$
  Beta(4,2) 
$$
was used as an prior distribution, since its support is [0,1].)

Unlike mle, since prior distribution of 
$$
  \theta
$$
was taken into consideration, posterior distribution of theta take more right skewed form than likelihood function.

<br>Of course, posterior distribution changes when 
1. Sample size changes
2. Prior distribution changes

<br>When sample size increases to 1000, for example, distributions take the following forms: 

<br>This makes sense because the relative affect of likelihood function increases as sample size increases. 

<br>Similarly, posterior distribution is more similar to prior distribution when sample size is small.

<br>Then what happens if prior distribution changes? Let's assume that we don't know prior distribution of theta so just take uniform distribution as prior.
Because prior distribution does not have much information about theta, posterior distribution derived consequently is similar to likelihood function.
