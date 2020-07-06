---
layout: post
title: "Probabilistic Record Linkage"
date: "2020-02-22"
description: "This post is a brief introduction to probabilistic record linkage"
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

To begin with, let's discuss about two types of record linkage:

- Deterministic record linkage
- Probabilistic record linkage

For deterministic record linkage, matching keys such as identification number and name, are used to integrate data. This is a practical way of integrating data, but at the same time it may miss matching pairs with typo errors. For example, for a pair with its name 'John Smith' and 'Jon Smith', these data will not be matched since 'John' and 'Jon' are not the same even if there is chance of being the same person. 

On the other hand, probabilistic record linkage uses characteristics of units, rather than *unique* matching keys. Probabilistic record linkage uses the following similarity score:
$$
R=\frac{P(\gamma|r \in M)}{P(\gamma|r \in U)}\;\;
$$
where
$$
\gamma: an\; agreement\; pattern
$$

$$
r: a\; pair\; of\; record
$$

$$
M: the\; set\; of\; true\; matches
$$

$$
U: the\; set\; of\; non-matches
$$

The above similarity score can be interpreted as the relative probability of agreement pattern, given that a pair is true match. Therefore, large value of $R$ implies that two records are from the same entity. 

In order to make decision based on $R$, probabilistic record linkage is implemented by the following procedure:

1.  For two distinct dataset $A$ and $B$(but from the same population), construct dataset with every possible combination of pairs; for dataset $A$ and $B$ with size of $n_{a}$ and $n_{b}$ respectively, dataset of cross product has size $n=n_{a}\cdot n_{b}$.

2. Calculate conditional probability for each pair. That is, 
   $$
   P(\gamma_{i}|r_{i} \in M)=\prod_{k=1}^{K}P(\gamma_{ik}|r_{i} \in M)=\prod_{k=1}^{K}m_{k}^{\gamma_{k}}(1-m_{k})^{1-\gamma_{k}}
   $$

   $$
   P(\gamma|r \in U)=\prod_{k=1}^{K}P(\gamma_{k}|r \in U)=\prod_{k=1}^{K}u_{k}^{\gamma_{k}}(1-u_{k})^{1-\gamma_{k}}
   $$

where
$$
\gamma_{i}=(\gamma_{i1},\gamma_{i2},\cdots \gamma_{iK})
$$
and 
$$
\gamma_{ik}|r_{i} \in M\; \sim\; B(1,m_{k})\; where\; m_{k}=P(\gamma_{ik}=1|r_{i} \in M)\; for\; i=1,2,\cdots ,n
$$

$$
\gamma_{ik}|r_{i} \in U\; \sim\; B(1,u_{k})\; where\; u_{k}=P(\gamma_{ik}=1|r_{i} \in U)\; for\; i=1,2,\cdots ,n
$$

3. Calculate similarity score $R=\frac{P(\gamma|r \in M)}{P(\gamma|r \in U)}\;\;$.

4. Make decision with by the following rule:
   - If $R\geq C_{U}$, a pair is *designated match*(*designated link*)
   - If $R\leq C_{U}$, a pair is *designated non-match*(*designated non-link*)
   - If $C_{L}<R<C_{U}$, a pair is *designated potential match*(*designated potential link*)



In order to calculate $R$, $m_{k}, u_{k}$ and $p=P(\gamma \in M)$ can be estimated using EM algorithm.

For the implementation of algorithm, we use the latent variable $g_{i}$ which takes the following form:
$$
g_{i}=\left\{\begin{matrix}
\;1\; if\; r_{i} \in M
\\ 
0\; if\; r_{i} \in U
\end{matrix}\right.
$$
Then, log likelihood function is:
$$
 logL(\theta;\gamma,g)
$$


 

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
