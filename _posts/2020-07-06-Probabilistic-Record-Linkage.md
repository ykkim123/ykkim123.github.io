---
layout: post
title: "Probabilistic Record Linkage"
date: "2020-07-07"
description: "This post explains probabilistic record linkage for data integration; step-by-step procedure for record linkage, and parameter estimation procedure for calculating similarity score"
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Data Integration
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

For deterministic record linkage, matching keys such as identification number and name, are used to integrate data. This is a practical way of integrating data, but may miss matching pairs due to misspelling. For example, for a pair of records with name 'John Smith' and 'Jon Smith' respectively, these records will not be matched since 'John' and 'Jon' are not the same even if there is a chance of being the same person.

On the other hand, probabilistic record linkage uses characteristics of records, rather than *unique* matching key. It uses the following similarity score: <br>


$$
R=\frac{P(\gamma|r \in M)}{P(\gamma|r \in U)}
$$


where <br>


$$
\begin{equation}
  \begin{array}{l}
    \gamma: an\; agreement\; pattern \\
    r: a\; pair\; of\; record \\
    M: a\; set\; of\; true\; matches \\
    U: a\; set\; of\; non-matches
  \end{array}
\end{equation}
$$



The above similarity score can be interpreted as relative probability of agreement pattern, given that a pair is true match. Therefore, large value of 
$$
R
$$
implies that two records are from the same entity. 

In order to make decision based on 
$$
R
$$
, probabilistic record linkage is implemented by the following procedure:

1. For two distinct dataset A and B(but from the same population), construct dataset with every possible combination of pairs. In other words, for dataset A and B with size of 
   $$
   n_{a}
   $$
   and 
   $$
   n_{b}
   $$
   respectively, construct dataset of cross product which has size of 
   $$
   n=n_{a}\cdot n_{b}
   $$
   .

2. Calculate conditional probability for each pair. That is, calculate <br>


   $$
   \begin{equation}
     \begin{array}{l}
   P(\gamma_{i}|r_{i} \in M)=\prod_{k=1}^{K}P(\gamma_{ik}|r_{i} \in M)=\prod_{k=1}^{K}m_{k}^{\gamma_{k}}(1-m_{k})^{1-\gamma_{k}} \\
   P(\gamma_{i}|r_{i} \in U)=\prod_{k=1}^{K}P(\gamma_{ik}|r_{i} \in U)=\prod_{k=1}^{K}u_{k}^{\gamma_{k}}(1-u_{k})^{1-\gamma_{k}}
     \end{array}
   \end{equation}
   $$

   where <br>

   $$
   \gamma_{i}=(\gamma_{i1},\gamma_{i2},\cdots \gamma_{iK})
   $$


   and <br>


   $$
   \begin{equation}
     \begin{array}{l}
   \gamma_{ik}|r_{i} \in M\; \sim\; B(1,m_{k})\; where\; m_{k}=P(\gamma_{ik}=1|r_{i} \in M)\; for\; i=1,2,\cdots ,N \\
   \gamma_{ik}|r_{i} \in U\; \sim\; B(1,u_{k})\; where\; u_{k}=P(\gamma_{ik}=1|r_{i} \in U)\; for\; i=1,2,\cdots ,N
     \end{array}
   \end{equation}
   $$

3. Calculate similarity score 
   $$
   R_{i}
   $$
   for each pair where <br>

   
   $$
   R_{i}=\frac{P(\gamma_{i}|r_{i} \in M)}{P(\gamma_{i}|r_{i} \in U)}
   $$
   .

4. Make decision by the following rule:

   - If 
     $$
     R_{i}\geq C_{U}
     $$
     , a pair is *designated match*(*designated link*)

   - If 
     $$
     R_{i}\leq C_{U}
     $$
     , a pair is *designated non-match*(*designated non-link*)

   - If 
     $$
     C_{L}<R_{i}<C_{U}
     $$
     , a pair is *designated potential match*(*designated potential link*)



In order to calculate 
$$
R_{i}
$$
, parameter 
$$
m_{k}, u_{k}
$$
 and 
$$
p=P(r_{i} \in M)
$$
 should be estimated, and this can be done by implementing EM algorithm.

First, we need to define latent variable 
$$
g_{i}
$$
 which takes the following form: <br>

$$
g_{i}=\left\{\begin{matrix}
\;1\; if\; r_{i} \in M
\\ 
0\; if\; r_{i} \in U
\end{matrix}\right.
$$


Then, log likelihood function can be written as: <br>


$$
\begin{equation}
  \begin{array}{l}
    logL(\theta;\gamma,g)=\prod_{i=1}^{N}(p \cdot P(\gamma_{i}|r_{i} \in M))^{g_{i}}((1-p) \cdot P(\gamma_{i}|r_{i} \in U))^{1-g_{i}} \\ 
    \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;=\prod_{i=1}^{N}(p \cdot \prod_{k=1}^{K}m_{k}^{\gamma_{k}}(1-m_{k})^{1-\gamma_{k}})^{g_{i}}((1-p) \cdot \prod_{k=1}^{K}u_{k}^{\gamma_{k}}(1-u_{k})^{1-\gamma_{k}})^{1-g_{i}}
  \end{array}
\end{equation}
$$


where <br>


$$
m=(m_{1},m_{2},\cdots ,m_{K}),\; u=(u_{1},u_{2},\cdots ,u_{K})
$$


and <br>


$$
\theta=(m,u,p)
$$


For E-step, latent variable 
$$
g_{i}
$$
 is updated as: <br>


$$
g_{i}^{(t)}=\frac{p^{(t)} \cdot \prod_{k=1}^{K}(m_{k}^{(t)})^{\gamma_{ik}}(1-m_{k}^{(t)})^{1-\gamma_{ik}}}{p^{(t)} \cdot \prod_{k=1}^{K}(m_{k}^{(t)})^{\gamma_{ik}}(1-m_{k}^{(t)})^{1-\gamma_{ik}}+(1-p^{(t)}) \cdot \prod_{k=1}^{K}(u_{k}^{(t)})^{\gamma_{ik}}(1-u_{k}^{(t)})^{1-\gamma_{ik}}}
$$


and for M-step, parameters are updated as below: <br>


$$
m_{k}^{(t+1)}=\frac{\sum_{i=1}^{N}g_{i}^{(t)}\gamma_{ik}}{\sum_{i=1}^{N}g_{i}^{(t)}}
$$

$$
u_{k}^{(t+1)}=\frac{\sum_{i=1}^{N}(1-g_{i}^{(t)})\gamma_{ik}}{\sum_{i=1}^{N}(1-g_{i}^{(t)})}
$$

$$
p^{(t+1)}=\frac{\sum_{i=1}^{N}g_{i}^{(t)}}{N}
$$



Here is an example for probabilistic record linkage. Below is the last 5 pairs of cross product for example data obtained from PPRL library.

| id.x  | gender.x | year.x | month.x | date.x | id.y  | gender.y | year.y | month.y | date.y |
| :---: | :------: | :----: | :-----: | :----: | :---: | :------: | :----: | :-----: | :----: |
| 12351 |    F     |  2008  |   12    |   14   | 12350 |    F     |  2008  |   14    |   12   |
| 12345 |    M     |  1964  |    1    |   1    | 12352 |    F     |  2008  |   12    |   14   |
| 12347 |    M     |  1987  |    3    |   30   | 12352 |    F     |  2008  |   12    |   14   |
| 12349 |    F     |  1972  |   10    |   24   | 12352 |    F     |  2008  |   12    |   14   |
| 12351 |    F     |  2008  |   12    |   14   | 12352 |    F     |  2008  |   12    |   14   |

<br>

Before implementing EM, we need to calculate 
$$
\gamma_{i}
$$

~~~r
N = nrow(data.cp)
gamma = list()

for (i in 1:N){
  
  temp = c()
  
  if (gender.x[i]==gender.y[i]){
    g=1}
  else {
    g=0}
  
  if (year.x[i]==year.y[i]){
    y=1}
  else {
    y=0}
  
  if (month.x[i]==month.y[i]){
    m=1}
  else {
    m=0}
  
  if (date.x[i]==date.y[i]){
    d=1}
  else {
    d=0}
  
  gamma[[i]] = c(g,y,m,d)
}
~~~

<br>

and initialize some values.

~~~r
K=4

m = rep(0.7,K)
u = rep(0.3,K)
p = 0.5
eta = c(m,u,p)
eta.list = eta
~~~

<br>

Now, we can implement EM by running the below code. It updates latent variable 
$$
g_{i}
$$
and parameter 
$$
m,u,p
$$
 in order, and stops iteration when sum of squares for difference is less than 
$$
10^{-7}
$$
.

~~~r
repeat{
  eta.prev = eta
  
  # E-step: update g
  g = c()
  for (i in 1:N){
    
    m.temp = 1
    u.temp = 1
    
    for (k in 1:K){
      m.temp = m.temp * (m[k]^(gamma[[i]][k])) *((1-m[k])^(1-gamma[[i]][k]))
      u.temp = u.temp * (u[k]^(gamma[[i]][k])) *((1-u[k])^(1-gamma[[i]][k]))}
    
    g.i = (p*m.temp)/(p*m.temp + (1-p)*u.temp)
    g = append(g, g.i)}
  
  # M-step
  ## Update m
  m = c()
  for (k in 1:K){
    num.temp = 0
    for (i in 1:N){
      num.i = g[i] * gamma[[i]][k]
      num.temp = num.temp + num.i}
    
    m.k = num.temp/sum(g)
    m = append(m, m.k)}
  
  ## Update u
  u = c()
  for (k in 1:K){
    num.temp = 0
    for (i in 1:N){
      num.i = (1-g[i]) * gamma[[i]][k]
      num.temp = num.temp + num.i}
    
    u.k = num.temp/sum(1-g)
    u = append(u, u.k)}
  
  ## Update p
  p = c()
  p.temp = sum(g)/N
  p = append(p, p.temp)
  
  
  # Convergence criteria
  eta = c(m,u,p)
  eta.list = rbind(eta.list,eta)
  diff = (eta.prev-eta)^2
  if (sum(diff)<1e-7)
    break}
~~~

<br>

Then, using the estimated parameters, we can calculate similarity score.

~~~r
R = c()

for (i in 1:N){
  
  num = 1
  denom = 1
  
  for (k in 1:K){
    if (gamma[[i]][k]==1){
      p.num.temp = m[k]
      p.denom.temp = u[k]}
    else{
      p.num.temp = 1-m[k]
      p.denom.temp = 1-u[k]}
      
    num = num * p.num.temp
    denom = denom * p.denom.temp}
    
    R_i = num/denom
    R = append(R, R_i)}
~~~

![similarity-score]({{ site.urlimg }}/Probabilistic-Record-Linkage/similarity-score.png "N=1000")

Above result shows that similarity scores for 12th and 16th pair are large enough to conclude that each pair is from the same entity. Therefore, the matching result for original dataset can be shown as:

| id.x  | gender.x | year.x | month.x | date.x | id.y  | gender.y | year.y | month.y | date.y | matching |
| :---: | :------: | :----: | :-----: | :----: | :---: | :------: | :----: | :-----: | :----: | :------: |
| 12351 |    F     |  2008  |   12    |   14   | 12350 |    F     |  2008  |   14    |   12   |    1     |
| 12345 |    M     |  1964  |    1    |   1    | 12352 |    F     |  2008  |   12    |   14   |    0     |
| 12347 |    M     |  1987  |    3    |   30   | 12352 |    F     |  2008  |   12    |   14   |    0     |
| 12349 |    F     |  1972  |   10    |   24   | 12352 |    F     |  2008  |   12    |   14   |    0     |
| 12351 |    F     |  2008  |   12    |   14   | 12352 |    F     |  2008  |   12    |   14   |    1     |

