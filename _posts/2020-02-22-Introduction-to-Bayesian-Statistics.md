---
layout: post
title: "Introduction to Bayesian Statistics"
date: "2020-02-22"
description: "This post is a brief introduction to Bayesian statistics. Specifically, it deals with difference between Bayesian and frequentist, and how posterior distribution changes when sample size and prior distribution change."
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

Bayesian statistics is different from frequentist in several aspects.
<br><br>To illustrate difference between frequentist and Bayesian view, consider the following situation: 
<br>Person
$$
  A
$$
toss a coin, where
$$
  Y_{i}
$$
is a random variable that represents ith result of coin toss among N observations and follows a Bernoulli distribution, i.e.,
$$
  Y_{i} \sim Ber(\theta)
$$
that takes a value of either 1(head) or 0(tail). 
<br><br>In **frequentist** view, parameter 
$$
  \theta
$$
can be estimated by maximum likelihood estimator(*mle*), 
$$
  \hat{\theta} = \frac{\sum_{i=1}^{N}Y_{i}}{N},
$$
which maximizes likelihood function
$$
  P(\theta|Y).
$$
<br>
![Frequentist]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/Frequentist.png "Frequentist")
In **Bayesian** view however, since parameter
$$
  \theta
$$
is a random variable, the following formula can be derived:
<br>
$$
  P(\theta|Y)=\frac{P(Y|\theta)P(\theta)}{P(Y)} \propto P(Y|\theta)P(\theta)
$$
<br>
  where
$$
  P(Y|\theta)
$$
is likelihood function, 
$$
  P(\theta)
$$
is prior distribution, and 
$$
  P(\theta|Y)
$$
is posterior distribution.
<br><br>The posterior distribution can be viewed as weighted average of likelihood function, with prior distribution functioning as weight.
<br><br>Plus, if the posterior distribution is in the same probability distribution family as the prior probability distribution, 
the prior and posterior are called conjugate distributions, and the prior is called a conjugate prior for the likelihood function. 
<br><br>After some algebra, posterior distribution is calculated as:
$$
  P(\theta|Y) \sim Beta(a+\alpha,N-a+\beta)
$$
<br>
where 
$$
  a
$$
is the number of heads.
<br>
(Here, 
$$
  Beta(4,2) 
$$
was used as an prior distribution of 
$$
  \theta
$$
,since its support is [0,1])
<br>
![Bayesian]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/Bayesian.png "Bayesian")
<br>Since prior distribution is taken into consideration, maximum credibility of posterior distribution deviates from 0.4.
<br><br>Posterior distribution may change when:
1. Sample size changes
2. Prior distribution changes
<br><br>When sample size increases, the posterior distribution becomes more similar to likelihood function:
<br>
![N=1000]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/N=1000.png "N=1000")
This makes sense because the relative affect of likelihood function increases as sample size increases.
<br><br>Similarly, posterior distribution becomes more similar to prior distribution when sample size decreases.
<br>
![N=10]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/N=10.png "N=10")
<br>Then what happens if prior distribution changes? Let's assume that we don't know prior distribution of 
$$
  \theta
$$
so just take uniformative distribution, 
$$
  Beta(1,1)
$$
as prior.
<br><br>Because prior distribution does not have much information about 
$$
  \theta,
$$ 
posterior distribution derived consequently is similar to likelihood function.
<br>
![Uninformative_prior]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/Uninformative_prior.png "Uninformative_prior")
<br><br>**Summary**
<br>In this post, followings are covered: 
- Difference between frequentist and Bayesian
- How posterior distribution changes as other conditions change
