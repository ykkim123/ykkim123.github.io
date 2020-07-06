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
\gamma_{ik}|r_{i} \in M\; \sim\; B(1,m_{k})\; where\; m_{k}=P(\gamma_{ik}=1|r_{i} \in M)\; for\; i=1,2,\cdots ,N
$$

$$
\gamma_{ik}|r_{i} \in U\; \sim\; B(1,u_{k})\; where\; u_{k}=P(\gamma_{ik}=1|r_{i} \in U)\; for\; i=1,2,\cdots ,N
$$

3. Calculate similarity score $R=\frac{P(\gamma|r \in M)}{P(\gamma|r \in U)}\;\;$.

4. Make decision with by the following rule:
   - If $R\geq C_{U}$, a pair is *designated match*(*designated link*)
   - If $R\leq C_{U}$, a pair is *designated non-match*(*designated non-link*)
   - If $C_{L}<R<C_{U}$, a pair is *designated potential match*(*designated potential link*)



In order to calculate $R$, parameter $m_{k}, u_{k}$ and $p=P(\gamma_{i} \in M)$ can be estimated using EM algorithm.

For the implementation of algorithm, we use the latent variable $g_{i}$ which takes the following form:
$$
g_{i}=\left\{\begin{matrix}
\;1\; if\; r_{i} \in M
\\ 
0\; if\; r_{i} \in U
\end{matrix}\right.
$$
Then, log likelihood function can be written as:
$$
\begin{equation}
  \begin{array}{l}
    logL(\theta;\gamma,g)=\prod_{i=1}^{N}(p \cdot P(\gamma_{i}|r_{i} \in M))^{g_{i}}((1-p) \cdot P(\gamma_{i}|r_{i} \in U))^{1-g_{i}} \\ 
    \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;=\prod_{i=1}^{N}(p \cdot \prod_{k=1}^{K}m_{k}^{\gamma_{k}}(1-m_{k})^{1-\gamma_{k}})^{g_{i}}((1-p) \cdot \prod_{k=1}^{K}u_{k}^{\gamma_{k}}(1-u_{k})^{1-\gamma_{k}})^{1-g_{i}}
  \end{array}
\end{equation}
$$
where
$$
m=(m_{1},m_{2},\cdots ,m_{K}),\; u=(u_{1},u_{2},\cdots ,u_{K})
$$
and
$$
\theta=(m,u,p)
$$
For E-step, latent variable $g_{i}$ is updated as:
$$
g_{i}^{(t)}=\frac{p^{(t)} \cdot \prod_{k=1}^{K}(m_{k}^{(t)})^{\gamma_{ik}}(1-m_{k}^{(t)})^{1-\gamma_{ik}}}{p^{(t)} \cdot \prod_{k=1}^{K}(m_{k}^{(t)})^{\gamma_{ik}}(1-m_{k}^{(t)})^{1-\gamma_{ik}}+(1-p^{(t)}) \cdot \prod_{k=1}^{K}(u_{k}^{(t)})^{\gamma_{ik}}(1-u_{k}^{(t)})^{1-\gamma_{ik}}}
$$
and for M-step, each parameter is updated as below:
$$
m_{k}^{(t+1)}=\frac{\sum_{i=1}^{N}g_{i}^{(t)}\gamma_{ik}}{\sum_{i=1}^{N}g_{i}^{(t)}}
$$

$$
u_{k}^{(t+1)}=\frac{\sum_{i=1}^{N}(1-g_{i}^{(t)})\gamma_{ik}}{\sum_{i=1}^{N}(1-g_{i}^{(t)})}
$$

$$
p^{(t+1)}=\frac{\sum_{i=1}^{N}g_{i}^{(t)}}{N}
$$
