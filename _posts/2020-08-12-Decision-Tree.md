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

Let's start with k-d tree, whose main purpose is to partition a space so that nearest neighbor search can be done more efficiently. For better understanding, take a look at an example below:

![k-d tree]({{ site.urlimg }}/Decision-Tree/k-d tree.png)

where <br>


$$
\begin{equation}
  \begin{array}{l}
    U\; is\; divided\; by\; median\;\\
    U=A \cup B \\
    d(x,y): distance\; between\; x\; and\; y \\
    d_{w}: distance\; between\; a\; point\; and\; wall 
  \end{array}
\end{equation}
$$


<br>
Now, suppose we want to find the nearest neighbor of 
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


However, under k-d tree structure, we can find the nearest neighbor by the following procedure:

1. Find 
   $$
   x^{*}
   $$
   , the local nearest neighbor of 
   $$
   x_{t}
   $$
    in 
    $$
    B
    $$
    .

2. If 
   $$
   d_{w}>d(x_{t},x^{*})
   $$
   , 
   $$
   x^{*}
   $$
    is the global nearest neighbor of 
   $$
   x_{t}
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
    that satisfies 
   $$
   d(x_{t},{x}')<d(x_{t},x^{*})
   $$
   .

   
<br>
This implies that if 
$$
d_{w}>d(x_{t},x)
$$
, then you can reduce computation time for searching the nearest neighbor by half. 

Of course, deeper k-d tree works in a similar way:

![deep k-d tree]({{ site.urlimg }}/Decision-Tree/deep k-d tree.png)



1. Find 
   $$
   x^{*}
   $$
   , the local nearest neighbor of 
   $$
   x_{t}
   $$
    in 
   $$
   D
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


<br>
Thus, we can find the nearest neighbor 
$$
x^{*}
$$
, without computing distance between 
$$
x_{t}
$$
 and 
$$
x\in (A \cup C)
$$
!

![deep k-d tree2]({{ site.urlimg }}/Decision-Tree/deep k-d tree2.png)




<br>
Also, if you want to find k-nearest neighbors, compare 
$$
d_{w}
$$
and 
$$
\underset{x\in S_{k}}{max}(d(x_{t},x))
$$
, where 
$$
S_{k}
$$
 is the k-nearest neighbors of 
$$
x_{t}
$$
. 

However, the algorithm does not work well if

- $$
  k
  $$
   is large

- data is high dimensional

because it's uncommon to be 
$$
d_{w}>d(x_{t},x)
$$
 and thus we cannot take an advantage of k-d tree.




<br>
# Ball Tree

![ball tree]({{ site.urlimg }}/Decision-Tree/ball tree.png)

Ball tree is a modification of k-d tree; it partitions a space by balls, instead of boxes. Compared to k-d tree, the algorithm has an advantage that it is relatively robust to high dimensional data.

The sketch of partitioning of a space is:

1. Pick any 
   $$
   x_{t} \in S
   $$
   .

2. Take 
   $$
   x_{1}
   $$
    that satisfies 
   $$
   d(x_{t},x_{1})=\underset{x\in S}{max}(d(x_{t},x))
   $$
   .

3. Take 
   $$
   x_{2}
   $$
    that satisfies 
   $$
   d(x_{1},x_{2})=\underset{x\in S}{max}(d(x_{1},x))
   $$
   .

4. Partition a space by 
   $$
   med(x_{1},x_{2})
   $$
   .

5. Calculate 
   $$
   c_{i}
   $$
   , an average of points in subset
   $$
   S_{i}
   $$
   .

6. Draw a circle(ball) whose centroid is
   $$
   c_i
   $$
    and diameter is 
   $$
   \underset{x\in S_{i}}{max}(d(c_i,x))
   $$
   .


<br>
The procedure of finding the nearest neighbor is identical to that of k-d tree. However, unlike k-d tree, a point may belong to multiple balls. In that case, the algorithm starts finding the nearest neighbor from the ball whose centroid is the closest to 
$$
x_{t}
$$
, but you cannot prune other balls in such case.




<br>
# Decision Tree

## Decision Tree for Classification

The intuition of decision tree starts from k-d tree; decision tree finds the approximate nearest neighbors by omitting a backtracking step exists in k-d tree.

Now, suppose we want to predict a label of new data 
$$
x_{new}
$$
. Then, we can think of two cases where a terminal node consists of either

1. data with the a single label
2. data with multiple labels



In case 1, it's obvious that finding nearest neighbors is meaningless, since all the labels of the nearest neighbors are identical. In case 2, where multiple labels exist, we predict a label of 
$$
x_{new}
$$
 by majority voting; we predict a label of the new data as the label that has the highest proportion. 

![decision tree]({{ site.urlimg }}/Decision-Tree/decision tree.png)

The above illustration shows how it works. Using the decision tree, a male whose age is greater than 9.5 would be predicted to be dead, and a male whose age is less than or equal to 9.5 and who has less than 3 siblings would be predicted to be survived.

Now, let
$$
R_{m}
$$
 be a terminal node that satisfies <br>
 

$$
\begin{equation}
  \begin{array}{l}
    \bigcup_{m=1}^{M}R_{m}=\mathcal{F} \\
    R_{m}\cap R_{n}=\emptyset  \\
    R_{m}\; is\; constructed\; by\; binary\; split\; of\; predictors
  \end{array}
\end{equation}
$$

<br>
Then, the label of 
$$
x
$$
 is predicted to be <br>
 

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


<br>
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



$$
\hat{f}(x)=\sum_{m=1}^{M}c_{m}I(x\in R_{m})
$$

where 
$$
c_{m}=\bar{y}_{m}
$$

Then, the model would look like below:



Now, let's consider how to prune decision tree with respect to cost complexity. That is, we should control tree size 
$$
|T|
$$
in order to avoid overfitting. Parameters that are subject to optimization include:

- maximum depth
- the maximum number of terminal nodes
- the minimum number of samples that terminal node should have
- the minimum number of samples to be splitted

 The optimization can be one in either naive(e.g. grid search) or rigorous way, but it's hard to accomplish the optimal level in naive ways. Therefore, we will take a look at cost complexity pruning, a more rigorous way of doing the same thing. Cost complexity pruning prune decision tree by introducing penalized loss function <br>
$$
R_{\alpha}(T)=R(T)+\alpha |T|
$$
where <br>
$$
R(T)=\sum_{m=1}^{|T|} \sum_{x_{i}\in R_{m}(j,s)}(y_{i}-\bar{y}_{m})^{2}
$$
and 
$$
\alpha
$$
is the nonnegative tuning parameter that controls the trade-off between tree size and goodness of fit.

For cost complexity pruning, we should first make a sequence of trees 
$$
T^{0}\supseteq T^{1}\supseteq \cdots \supseteq T^{n}
$$
where <br>
$$
\begin{equation}
  \begin{array}{l}
  T^{n}: null\; tree \\
  T^{0}: full\; tree
  \end{array}
\end{equation}
$$
Also, denote
$$
\begin{equation}
  \begin{array}{l}
  R_{\alpha}(T_{t})=R(T_{t})+\alpha |T_{t}|:penalized\; loss\; of \; T_{t} \\
  T_{t}:subtree\; of\; T_{0}\; that\; takes\; node\; t\; as\; root \\
  R_{\alpha}(t)=R(t)+\alpha: penalized\; loss\; of\; node\; t \\
  \end{array}
\end{equation}
$$
and take a look at the graph below. 

![cost complexity pruning]({{ site.urlimg }}/Decision-Tree/cost complexity pruning.png)



Without penalty term(
$$
\alpha=0
$$
), it's obvious that 
$$
T_{t}
$$
 has smaller loss than 
$$
t
$$
, which is a single node. However, as 
$$
\alpha
$$
increases, penalty for making deeper tree increases, and after certain level(
$$
{\alpha}'=\frac{R(t)-R(T_{t})}{|T_{t}|-1}
$$
), the relationship between 
$$
R_{\alpha}(T_{t})
$$
 and 
$$
R_{\alpha}(t)
$$
 reverses. Therefore, 
$$
{\alpha}'
$$
can be viewed as a level beyond which loss is greater than gain, when you make a deeper tree from root node 
$$
t
$$
.

Now, suppose that 
$$
\alpha_{t_{k}}
$$
 has the smallest value among all 
$$
{\alpha}'
$$
s. Then, node 
$$
t_{k}
$$
is called **the weakest link** and tree is updated as: <br>
$$
T^{1}=T^{0}-T^{0}_{t_{k}}
$$
while tuning parameter 
$$
\alpha
$$
is also updated as:
$$
\alpha_{1}=\frac{R(t_{k})-R(T^{0}_{t_{k}})}{|T^{0}_{t_{k}}|-1}
$$
That is, 
$$
T_{1}
$$
is the optimal tree that minimizes 
$$
R_{\alpha_{1}}(T)
$$
.

To summarize, cost-complexity pruning is implemented by the following procedure:

- Initialization

  - $$
    T_{0}
    $$

    : a full tree

  - $$
    \alpha_{0}=0
    $$

- For 
  $$
  s=1,2,\cdots
  $$
    , do:

  - Select the node 
    $$
    t
    $$
     that minimizes 
    $$
    \frac{R(t)-R(T_{t}^{s-1})}{|T^{s-1}_{t}|-1}
    $$

  - Set 
    $$
    T^{s}=T^{s-1}-T^{s-1}_{t}
    $$
     and 
    $$
    \alpha_{s}=\frac{R(t)-R(T_{t}^{s-1})}{|T^{s-1}_{t}|-1}
    $$

After Iterating the algorithm until it reaches
$$
T_{n}
$$
, then we obtain a sequence of trees 
$$
T^{0}\supseteq T^{1}\supseteq \cdots \supseteq T^{n}
$$
and a non-decreasing sequence of tuning parameters
$$
\alpha_{0}\leq \alpha_{1}\leq \cdots \leq \alpha_{n}
$$


Based on the above result, optimal value of 
$$
\alpha^{*}
$$
 can be obtained by cross validation:

- Initialization

  - $$
    D^{(v)}=D-D_{v}
    $$

    where 
    $$
    v\in \left \{1,2,\cdots,V \right\}
    $$

  - $$
    \alpha \in \left \{\alpha_{1}',\alpha_{2}',\cdots,\alpha_{n-1}' \right\}
    $$

     where 
    $$
    \alpha_{k}'=\sqrt{\alpha_{k} \alpha_{k+1}}
    $$

- For 
  $$
  v=1,2,\cdots ,V
  $$
  , do:

  - Make sequences <br>
    $$
    T^{(v,0)}\supseteq T^{(v,1)}\supseteq \cdots \supseteq T^{(v,n)}
    $$

    $$
    \alpha_{0}'\leq \alpha_{1}'\leq \cdots \leq \alpha_{n-1}'
    $$

    from 
    $$
    D^{(v)}
    $$

- For 
  $$
  k=1,2,\cdots, n-1
  $$
  , compute cross validation error and choose one among 
  $$
  {\alpha_{1}}',{\alpha_{1}}',\cdots, {\alpha_{n-1}}'
  $$
   that minimizes the error.

The above pruning methods are also valid for classification tree; just use 
$$
R(T)
$$
 based on Gini impurity or entropy instead.

And some characteristics of regression tree are:

- Easy to capture the overall pattern
- Useful than linear regression, especially when non-linear relationship exists
- Easy to deal with categorical features
- Discontinuities exist in split points





# Implementation using Python

## Decision Tree for Classification

For classification problem, we will use iris dataset: 

~~~python
from sklearn.datasets import load_iris

X, y = load_iris(return_X_y=True)
feature_names = load_iris().feature_names
class_names = load_iris().target_names
~~~

<br>

We can make decision tree for classification using *DecisionTreeClassifier* in *sklearn*

~~~python
from sklearn.tree import DecisionTreeClassifier

clf = DecisionTreeClassifier(criterion='entropy', splitter='best', max_depth=5, min_samples_split=50, min_samples_leaf=50, 
                             random_state=0, max_leaf_nodes=10)
~~~

where 

- max_depth: maximum depth
- max_leaf_nodes: the maximum number of terminal nodes
- min_samples_leaf: the minimum number of samples that terminal node should have
- min_samples_split: the minimum number of samples to be splitted



and visualize the result:

~~~python
import io
import pydot
from IPython.core.display import Image 
from sklearn.tree import export_graphviz

def draw_decision_tree(model, feature_names=None, class_names=None, proportion=False):
    dot = export_graphviz(model, feature_names=feature_names, class_names=class_names, node_ids=True, proportion=proportion,
                          filled=True)
    graph = pydot.graph_from_dot_data(dot)[0]
    image = graph.create_png()
    return Image(image)
~~~

~~~python
tree = clf.fit(X, y)
draw_decision_tree(tree, feature_names, class_names)
~~~

![classification tree]({{ site.urlimg }}/Decision-Tree/classification tree.png)



And for evaulation of the model, we can use *cross_val_score* function

~~~python
from sklearn.model_selection import cross_val_score

res = cross_val_score(clf, X, y, cv=5)
res.mean() #Output is 0.6599999999999999
~~~

or compute a confusion matrix:

~~~python
from sklearn.metrics import confusion_matrix

cf_iris = pd.DataFrame(confusion_matrix(y, tree.predict(X)), index=class_names, columns=class_names)
cf_iris
~~~

| True/Predicted | setosa | versicolor | virginica |
| :------------: | :----: | :--------: | :-------: |
|     setosa     |   50   |     0      |     0     |
|   versicolor   |   0    |     50     |     0     |
|   virginica    |   0    |     50     |     0     |

<br>

Also, you can check importance of each feature:

~~~python
importances = pd.Series(tree.feature_importances_, index=feature_names)
importances
~~~

|      Feature      | Importance |
| :---------------: | :--------: |
| sepal length (cm) |    0.0     |
| sepal width (cm)  |    0.0     |
| petal length (cm) |    0.0     |
| petal width (cm)  |    1.0     |

which returns values based on the (normalized) total reduction of the criterion brought by that feature.

Now, in order to optimize hyperparameters, let's start with grid search

~~~python
from sklearn.model_selection import GridSearchCV

hyperparamters = {'max_depth':list(range(2, 10)), 'max_leaf_nodes':list(range(10,20)), 'min_samples_leaf':list([40,50,60]),
                  'min_samples_split':list([40,50,60])}
GridCV = GridSearchCV(estimator=tree, param_grid=hyperparamters, cv=5) 
GridCV.fit(X,y)
GridCV.best_params_
~~~

which has the following result:

|      Feature      | Optimal Value |
| :---------------: | :-----------: |
|     max_depth     |       2       |
|  max_leaf_nodes   |      10       |
| min_samples_leaf  |      40       |
| min_samples_split |      40       |



And optimized tree by grid search can be illustrated as below:

![classification tree2]({{ site.urlimg }}/Decision-Tree/classification tree2.png)



Also, we can optimize hyperparameters by cost complexity pruning. First, we can obtain a sequence of 
$$
\alpha
$$
 and corresponding impurity:

|   Alpha    |  Impurity  |
| :--------: | :--------: |
|     0      |     0      |
| 0.01836592 | 0.03673183 |
| 0.01836592 | 0.05509775 |
| 0.02797049 | 0.08306824 |
| 0.04675016 | 0.1298184  |
| 0.0767413  | 0.20655975 |
| 0.46010691 | 0.66666667 |
| 0.91829583 | 1.5849625  |

<br>

and make a geometric sequence of 
$$
\alpha
$$
 based on the previous result.

~~~python
alphas = []
for i in range(len(cc_pruning['ccp_alphas'])-1):
    temp = np.sqrt(cc_pruning['ccp_alphas'][i]*cc_pruning['ccp_alphas'][i+1])
    alphas.append(temp)
~~~

<br>

Using the geometric sequence, we can obtain accuracy by cross validation.

|  Alpha   | Accuracy |
| :------: | :------: |
| 0.000000 | 0.953333 |
| 0.018366 | 0.953333 |
| 0.022665 | 0.953333 |
| 0.036161 | 0.960000 |
| 0.059897 | 0.946667 |
| 0.187907 | 0.933333 |
| 0.650011 | 0.666667 |

Thus, we can find the optiaml value of 
$$
\alpha
$$
: <br>
$$
\alpha^{*}=0.650011
$$
and construct a pruned tree based on the result: 

![classification tree3]({{ site.urlimg }}/Decision-Tree/classification tree3.png)



The result so far can be summarized as:

|           Hyperparameters            | Accuracy |
| :----------------------------------: | :------: |
|        Random hyperparameter         |  0.660   |
|       Optimized by grid search       |  0.767   |
| Optimized by cost complexity pruning |  0.960   |

<br>

Although grid search increases accuracy, hyperparameters are only local optimum obtained by some candidates we assigned in advance. By cost complexity pruning, on the other hand, we can attain much higher level of accuracy by controlling tuning parameter
$$
\alpha
$$
.



Similarly, you can build a regression tree, using *DecisionTreeRegressor*:

~~~python
from sklearn.tree import DecisionTreeRegressor

reg = DecisionTreeRegressor(criterion='mse', splitter='best', max_depth=2, min_samples_split=50, min_samples_leaf=50, random_state=0, 
                            max_leaf_nodes=10)
~~~

![regression tree]({{ site.urlimg }}/Decision-Tree/regression tree.png)



Unlike classification tree, regression tree

- uses MSE as an criterion
- splits a node by dividing into intervals



and remaining things can be done in a similar way to what we did for classification tree(check more detail here).

