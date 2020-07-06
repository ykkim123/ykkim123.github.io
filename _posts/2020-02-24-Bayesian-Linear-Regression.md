---
layout: post
title: "Introduction to Bayesian Statistics"
date: "2020-02-22"
description: "This post is a brief introduction to Bayesian statistics; specifically, it's about difference between Bayesian and frequentist, and how posterior distribution changes when sample size and prior distribution change."
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Bayesian
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
---

Bayesian statistics is different from frequentist in several aspects. To illustrate difference between frequentist and Bayesian view, consider the following situation: 
<br><br>Person
$$
  A
$$
toss a coin, where
$$
  Y_{i}
$$
is a random variable that represents ith result of coin toss and takes a value of either 1(head) or 0(tail). Also, assume
<br><br>
$$
  Y_{i} \sim Ber(\theta)
$$ 
<br><br><br>
In **frequentist** view, 
$$
  \theta
$$
can be estimated by maximum likelihood estimator(*mle*), 
<br><br>
$$
  \hat{\theta} = \frac{\sum_{i=1}^{N}Y_{i}}{N},
$$
<br><br>
which maximizes likelihood function
$$
  P(\theta|Y).
$$
<br>
![Frequentist]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/Frequentist.png "Frequentist")
<br>In **Bayesian** view however, since
$$
  \theta
$$
is a random variable, the following formula can be derived:
<br><br>
$$
  P(\theta|Y)=\frac{P(Y|\theta)P(\theta)}{P(Y)} \propto P(Y|\theta)P(\theta)
$$
<br><br>
  where
$$
  P(Y|\theta)
$$
is likelihood function, 
$$
  P(\theta)
$$
is called prior distribution, and 
$$
  P(\theta|Y)
$$
is called posterior distribution. 
<br><br>After some algebra, posterior distribution is calculated as:
<br><br>
$$
  P(\theta|Y) \sim Beta(a+\alpha,N-a+\beta)
$$
<br><br>
where 
$$
  a
$$
is the number of heads.
<br>
(In this example, 
$$
  Beta(4,2) 
$$
was used as a prior distribution of 
$$
  \theta
$$
)<br>
![Bayesian]({{ site.urlimg }}/Introduction-to-Bayesian-Statistics/Bayesian.png "Bayesian")
<br>Since prior distribution is taken into consideration, maximum credibility of posterior distribution deviates from 0.4.
<br><br><br>Posterior distribution may change when:
- Sample size changes
- Prior distribution changes

<br>When sample size increases, the posterior distribution becomes more similar to likelihood function:
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
<br>In this post, followings were covered: 
- Difference between frequentist and Bayesian
- How posterior distribution changes as sample size and prior distribution change
