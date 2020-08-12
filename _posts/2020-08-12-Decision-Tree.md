---
layout: post
title: "Decision Tree"
date: "2020-08-12"
description: "This post explains decision tree algorithm; introduction of k-d tree and ball tree, and mathematical explanation for decision tree for evaluation of the model."
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Machine Learning
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false

---

# k-d Tree

First, let's discuss about k-d tree, whose main purpose is to partition a space so that nearest neighbor search can be done more efficiently. For better understanding consider the following example:

![k-d tree]({{ site.urlimg }}/Decision-Tree/k-d tree.png)

where <br>
$$
\begin{equation}
  \begin{array}{l}
    U\; is\; divided\; by\; median\\
    U=A \cup B \\
    d(x,y): distance\; between\; x\; and\; y \\
    d_{w}: distance\; between\; a\; point\; and\; wall 
  \end{array}
\end{equation}
$$



Now, suppose that we want to find the nearest neighbor of 
$$
x_{t}
$$
. Without k-d tree structure, we have to calculate every
$$
d(x_{t},x)
$$
for  
$$
(n-1)
$$
 points.

Next, let's look at the same problem under k-d tree structure. We can find the nearest neighbor by the following procedure:

1. Find 
   $$
   x^{*}
   $$
   , the nearest neighbor of 
   $$
   x_{t}
   $$
    such that 
   $$
   x^{*}\in B
   $$

2. If 
   $$
   d_{w}>d(x_{t},x^{*})
   $$
   , 
   $$
   x^{*}
   $$
    is the nearest neighbor of 
   $$
   x_{t}
   $$
   in 
   $$
   U
   $$
   . Otherwise, calculate 
   $$
   d(x_{t},x)
   $$
    for 
   $$
   x \in A
   $$
    and update 
   $$
   x^{*}
   $$
    if you find 
   $$
   {x}'
   $$
    such that 
   $$
   d(x_{t},{x}')<d(x_{t},x^{*})
   $$
   .

   

This implies that if 
$$
d_{w}>d(x_{t},x)
$$
is satisfied, then you can reduce time taken for searching the nearest neighbor by half. 

You can do the similar thing for deeper k-d tree:

![deep k-d tree]({{ site.urlimg }}/Decision-Tree/deep k-d tree.png)



Similarly, the algorithm finds the nearest neighbor of 
$$
x_{t}
$$
in the following order:

1. Find 
   $$
   x^{*}
   $$
   , the nearest neighbor of 
   $$
   x_{t}
   $$
    such that 
   $$
   x^{*}\in D
   $$
   .

2. Since 
   $$
   d(x_{t},t_{3})<d(x_{t},x^{*})
   $$
   , we cannot prune 
   $$
   B
   $$
   . Thus, repeat step
   $$
   1
   $$
   for 
   $$
   B
   $$
   and update 
   $$
   x^{*}
   $$
   if you find a closer point in 
   $$
   B
   $$
   .

3. Since 
   $$
   d(x_{t},t_{1})>d(x_{t},x^{*})
   $$
   , we can prune the subtree 
   $$
   A \cup C
   $$

.

Now, we found the nearest neighbor 
$$
x^{*}
$$
, without calculating distance between 
$$
x_{t}
$$
and points in 
$$
A \cup C
$$
! The result so far can be illustrated by the dendrogram below:

![deep k-d tree2]({{ site.urlimg }}/Decision-Tree/deep k-d tree2.png)





Of course, for finding k-nearest neighbors, simply compare 
$$
d_{w}
$$
and 
$$
\underset{x\in S_{k}}{max}(d(x_{t},x))
$$
 where 
$$
S_{k}: k-nearest\; neighbors\; of\; x_{t}
$$
. 

However, the algorithm does not work well when

- $$
  k
  $$

   is large

- data is high dimensional

because in above cases, it's uncommon to be 
$$
d_{w}>d(x_{t},x)
$$
 and thus cannot take the advantage of k-d tree.





# Ball Tree

![ball tree]({{ site.urlimg }}/Decision-Tree/ball tree.png)

Ball tree is a modification of k-d tree; specifically, it partitions a space by balls, not by boxes. Compared to k-d tree, the algorithm has an advantage that is relatively robust to high dimensional data.

It partitions a space by the following procedure:

1. Pick any 
   $$
   x_{t}
   $$
   in 
   $$
   S
   $$
   .
   
2. Take 
   $$
   x_{1}
   $$
    such that 
   $$
   d(x_{t},x_{1})=\underset{x\in S}{max}(d(x_{t},x))
   $$
   .

3. Take 
   $$
   x_{2}
   $$
    such that 
   $$
   d(x_{1},x_{2})=\underset{x\in S}{max}(d(x_{1},x))
   $$
    for all 
   $$
   x
   $$
   .

4. Partition a space by 
   $$
   m=med(x_{1},x_{2})
   $$
   .

5. For each partitioned subset, calculate 
   $$
   c_{i}
   $$
   , an average of points in the subset.

6. Draw a circle(ball) whose centroid and radius are
   $$
   c_i
   $$
    and
   $$
   \underset{x\in S_{i}}{max}(d(c_i,x))
   $$
   , respectively.



The procedure for finding the nearest neighbor is identical to k-d tree. However, unlike k-d tree, a point may belong to multiple balls. In that case, the algorithm starts to find the nearest neighbor from the ball whose centroid is the closest to 
$$
x_{t}
$$
, but you cannot prune other balls in such case.





# Decision Tree

## Decision Tree for Classification

The intuition of decision tree starts from k-d tree. In k-d tree, there is a backtracking step to check if the nearest neighbors we found locally are global nearest neighbors. In decision tree, however, we find the approximate nearest neighbors by omitting this step.

Now, suppose we want to predict a label of new data 
$$
x_{t}
$$
. Then, we can think of two cases where a terminal node consists of 

1. data with the a single label
2. data with multiple labels



In case 1, it's obvious that finding nearest neighbors is meaningless, since labels of the nearest neighbors are identical. In case 2, where multiple labels exist, we determine a label of 
$$
x_{t}
$$
 by majority voting; i.e. we predict a label of the new data as the label that has the highest proportion in that node. 

![decision tree]({{ site.urlimg }}/Decision-Tree/decision tree.png)

The above illustration shows how this works. From the decision tree, a male whose age is greater than 9.5 would be predicted to be dead, while a male whose age is less than or equal to 9.5 and who has less than 3 siblings would be predicted to be survived.

For rigorous explanation, define a terminal node 
$$
R_{m}
$$
 that satisfies <br>
$$
\begin{equation}
  \begin{array}{l}
    \bigcup_{m=1}^{M}R_{m}=\mathcal{F} \\
    R_{m}\cap R_{n}=\emptyset  \\
    R_{m}\; is\; constructed\; by\; binary\; split\; of\; predictors
  \end{array}
\end{equation}
$$

Then, the label of 
$$
x
$$
 is predicted to be: <br>
$$
\hat{f}(x)=\sum_{m=1}^{M}c_{m}I(x\in R_{m})
$$
where 
$$
\begin{equation}
  \begin{array}{l}
  c_{m}=\underset{c_{k}}{argmax}(p_{mk}) \\
  p_{mk}=the\; proportion\; of\; class\; k\; in\; R_{m}
  \end{array}
\end{equation}
$$



Now, let's consider how to evaluate decision tree. A misclassification rate of tree 
$$
T
$$
 is:
$$
E(T)=\frac{\sum_{m=1}^{|T|}\sum_{x_{i}\in R_{m}}I(y_{i}\neq c_{m})}{N}
$$
where 
$$
\begin{equation}
  \begin{array}{l}
  |T|:the\; number\; of\; terminal\; nodes \\
  N_{m}=the\; number\; of\; samples\; in\; R_{m} \\
  N=\sum_{m=1}^{|T|}N_{m}
  \end{array}
\end{equation}
$$



Impurity functions, which are much more commonly used, measure to which extent the node is "impure", with respect to proportion of labels. From the previous illustration, it's obvious that we want to make this impurity as low as possible.

First, gini impurity of terminal node 
$$
t
$$
is defined as: <br>
$$
G(t)=\sum_{k=1}^{K}p_{mk}(1-p_{mk})
$$
and subtree 
$$
t_{i}
$$
 consists of terminal nodes 
$$
t_{i1}
$$
 and 
$$
t_{i2}
$$
 has a weighted mean of 
$$
G(t_{i1})
$$
 and 
$$
G(t_{i2})
$$
 as gini impurity, i.e. <br>
$$
G(t_{i})=\frac{N_{t_{1}}}{N_{t_{i}}}G(t_{i1})+\frac{N_{t_{2}}}{N_{t_{i}}}G(t_{i2})
$$


Next, to discuss about entropy, let's start with Kullback-Leibler divergence defined as: <br>
$$
KL_{t}(p\parallel q)=\sum_{k=1}^{K}p_{k}log\frac{p_{k}}{q_{k}}
$$
that computes closeness between 
$$
p
$$
 and 
$$
q
$$
. 

Now, for the understanding of entropy, think of the most "impure" case where the probabilities for each label are identical, i.e. <br>
$$
q_{k}=\frac{1}{K}\; for\; k=1,2,\cdots ,K
$$
Then, Kullback-Leibler divergence from 
$$
q
$$
 to 
$$
p
$$
is defined as: <br>
$$
\begin{equation}
  \begin{array}{l}
  KL_{t}(p\parallel q)=\sum_{k=1}^{K}(p_{k}log(p_{k})+p_{k}logK) \\
  \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;=\sum_{k=1}^{K}(p_{k}log(p_{k}))+logK
  \end{array}
\end{equation}
$$
Since 
$$
logK
$$
 is a constant, we want to maximize 
$$
\sum_{k=1}^{K}(p_{k}log(p_{k}))
$$
 which is equivalent to minimizing entropy: <br>
$$
H(t)=-\sum_{k=1}^{K}(p_{k}log(p_{k}))
$$
and entropy of subtree 
$$
t_{i}
$$
is computed in a similar way: <br>
$$
H(t_{i})=\frac{N_{t_{1}}}{N_{t_{i}}}H(t_{i1})+\frac{N_{t_{1}}}{N_{t_{i}}}H(t_{i2})
$$



The following graph shows 3 different evaluation measures when 
$$
K=2
$$
.

![loss function]({{ site.urlimg }}/Decision-Tree/loss function.png)

You can check that:

- For numerical optimization, missclassification error is not a good option, since it is not differentiable

- Entropy>Gini impurity>Missclassification error(in terms of sensitivity to impurity)

  

Then, for the loss function 
$$
R(t)
$$
 chosen among such loss functions, the algorithm finds the optimal split for node 
$$
t
$$
by finding the best pair of some variable 
$$
j
$$
and split point 
$$
s
$$
 that satisfies the following: <br>
$$
(j^{*},s^{*})=\underset{(j,s)}{argmin}\Delta(j,s)
$$
where 
$$
\Delta(j,s)
$$
 is defined by a difference between current loss and loss after splitting, i.e. <br>
$$
\Delta(j,s)=R(t)-(\frac{N_{t_{1}}}{N_{t}}R(t_{1})+\frac{N_{t_{2}}}{N_{t}}R(t_{2}))
$$
The following procedure is done by greedy search. That is, 

- For 
  $$
  d_{n}
  $$
  numerical variables that have
  $$
  n
  $$
  unique values respectively, the algorithm tries every possible
  $$
  d_{n}\cdot n
  $$
  split

- For 
  $$
  d_{m}
  $$
  categorical variables that have
  $$
  m
  $$
  unique values respectively, the algorithm tries every possible
  $$
  d_{m}\cdot (2^{m-1}-1)
  $$
  split



And here are some characteristics that decision tree has:

- Robust to scale of features

- Computation is fast

  - Only store data of terminal nodes

  - $$
    O(DKN)
    $$

    complexity where <br>
    $$
    \begin{equation}
      \begin{array}{l}
      D:the\; number\; of \; dimensions \\
      K:the\; number\; of\; classes \\
      N:the\; number\; of \; samples\;
      \end{array}
    \end{equation}
    $$

- Easy to interpret
- Any form of data type is possible
- Robust to outliers/missing values



## Decision Tree for Regression

Decision tree for regression has similar structure to that of classification. However, it computes prediction by <br>



