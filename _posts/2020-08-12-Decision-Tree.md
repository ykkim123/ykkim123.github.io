---
layout: post
title: "Decision Tree"
date: "2020-08-12"
description: "This post explains decision tree algorithm; introduction of k-d tree/ball tree, and mathematical explanation of decision tree with evaluation of the model."
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

where

$$
\begin{equation}
  \begin{array}{l}
    U=A \cup B\; is\; divided\; by\; median\;\\
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
. Without k-d tree structure, we have to calculate 
$$
d(x_{t},x)
$$
for every   
$$
(n-1)
$$
 point.

However, under k-d tree structure, we can find the nearest neighbor by the following procedure:

1. Find 
   $$
   x^{*}
   $$
   , the nearest neighbor of 
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
   , the nearest neighbor of 
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

The sketch of partitioning a space 
$$
S
$$
 is:

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
    and radius is 
   $$
   \underset{x\in S_{i}}{max}(d(c_i,x))
   $$
   .

<br>
The procedure of finding the nearest neighbor is identical to that of k-d tree. However, unlike k-d tree, a point may belong to multiple balls. In that case, the algorithm starts finding the nearest neighbor from the ball whose centroid is closest to 
$$
x_{t}
$$
, but you cannot prune other balls in such case.




<br>
# CART

## Decision Tree for Classification

The intuition of decision tree starts from k-d tree; decision tree finds approximate nearest neighbors by omitting a backtracking step exists in k-d tree.

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
After fitting a tree, evaluation the model should be done in a proper manner. A misclassification rate of tree 
$$
T
$$
 is defined as:
 

$$
E(T)=\frac{\sum_{m=1}^{|T|}\sum_{x_{i}\in R_{m}}I(y_{i}\neq c_{m})}{N}
$$

where

$$
\begin{equation}
  \begin{array}{l}
  |T|=the\; number\; of\; terminal\; nodes \\
  N_{m}=the\; number\; of\; samples\; in\; R_{m} \\
  N=\sum_{m=1}^{|T|}N_{m}
  \end{array}
\end{equation}
$$


<br>
Impurity functions, which are more commonly used, measure to which extent the node is "impure", with respect to the proportion of labels.

First, gini impurity of terminal node 
$$
t
$$
is defined as

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
 has gini impurity
 
$$
G(t_{i})=\frac{N_{t_{1}}}{N_{t_{i}}}G(t_{i1})+\frac{N_{t_{2}}}{N_{t_{i}}}G(t_{i2})
$$

<br>
Next, let's start from Kullback-Leibler divergence defined as:

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

Now, think of the most "impure" case where the probabilities for every label are identical, i.e.

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
is defined as

$$
\begin{equation}
  \begin{array}{l}
  KL_{t}(p\parallel q)=\sum_{k=1}^{K}(p_{k}log(p_{k})+p_{k}logK) \\
  \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;=\sum_{k=1}^{K}(p_{k}log(p_{k}))+logK
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
, which is equivalent to minimizing entropy

$$
H(t)=-\sum_{k=1}^{K}(p_{k}log(p_{k}))
$$

Also, subtree version of entropy is defined similarly:

$$
H(t_{i})=\frac{N_{t_{1}}}{N_{t_{i}}}H(t_{i1})+\frac{N_{t_{1}}}{N_{t_{i}}}H(t_{i2})
$$


<br>
The following graph shows 3 different loss functions when 
$$
K=2
$$
.

![loss function]({{ site.urlimg }}/Decision-Tree/loss function.png)

You can see that:

- For the purpose of numerical optimization, misclassification error is not a good option, since it is not differentiable

- Entropy>Gini impurity>Missclassification error, in terms of sensitivity to impurity

  
<br>
For loss function 
$$
R(t)
$$
, the algorithm finds the optimal split of node 
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
 that satisfies

$$
(j^{*},s^{*})=\underset{(j,s)}{argmax}(\Delta(j,s))
$$

where

$$
\Delta(j,s)=R(t)-(\frac{N_{t_{1}}}{N_{t}}R(t_{1})+\frac{N_{t_{2}}}{N_{t}}R(t_{2}))
$$

is a difference between current loss and loss after splitting.

Finding
$$
(j^{*},s^{*})
$$
 is done by greedy search. That is, 

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
  (d_{n}\cdot n) 
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


<br>
## Decision Tree for Regression

Decision tree for regression has similar structure to that of classification. However, it computes prediction by

$$
\hat{f}(x)=\sum_{m=1}^{M}c_{m}I(x\in R_{m})
$$

where 
$$
c_{m}=\bar{y}_{m}
$$
.

Then, regression tree would look like below:
![regression tree]({{ site.urlimg }}/Decision-Tree/regression tree.png)


<br>
Let's consider how to prune regression tree with respect to cost complexity. That is, we want to control tree size 
$$
|T|
$$
 to avoid overfitting. 

Finding the optimal 
$$
|T|
$$
 can be done in a naive(e.g. grid search) way, but it's hard to find the optimal value. Therefore, cost complexity pruning, which is a more rigorous way of optimizing hyperparameters, is often more helpful. The algorithm prunes a tree by using penalized loss function

$$
R_{\alpha}(T)=R(T)+\alpha |T|
$$

where

$$
R(T)=\sum_{m=1}^{|T|} \sum_{x_{i}\in R_{m}}(y_{i}-\bar{y}_{m})^{2}
$$

and 
$$
\alpha
$$
is a nonnegative tuning parameter that controls the trade-off between tree size and goodness of fit.

For cost complexity pruning, we should first obtain a sequence of trees

$$
T^{0}\supseteq T^{1}\supseteq \cdots \supseteq T^{n}
$$

where 
$$
T^{0}
$$
 is a fully grown tree and 
$$
T^{n}
$$
 is a null tree.

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

and take a look at the graph below:

![cost complexity pruning]({{ site.urlimg }}/Decision-Tree/cost complexity pruning.png)


<br>
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
increases, penalty for making deeper tree also increases, and after certain level(
$$
{\alpha}'
$$
), the relationship between 
$$
R_{\alpha}(T_{t})
$$
 and 
$$
R_{\alpha}(t)
$$
 reverses. Thus, 
$$
{\alpha}'
$$
can be interpreted as a level beyond which loss is greater than gain, when you make a tree from root node 
$$
t
$$
.

For 
$$
T^{0}
$$
, suppose that 
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
is called the **weakest link** and tree is updated as

$$
T^{1}=T^{0}-T^{0}_{t_{k}}
$$

while tuning parameter 
$$
\alpha
$$
is also updated as

$$
\alpha_{1}=\frac{R(t_{k})-R(T^{0}_{t_{k}})}{|T^{0}_{t_{k}}|-1}
$$

That is, 
$$
T^{1}
$$
is the optimal tree that minimizes 
$$
R_{\alpha_{1}}(T^{0})
$$
.

Thus, cost-complexity pruning can be implemented in the following order: 

- Initialization

  - $$
    T^{0}: a\; full\; tree
    $$
      
  - $$
    \alpha_{0}=0
    $$

- For 
  $$
  s=1,2,\cdots
  $$
  , do:

  - Find the node 
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

<br>
After iterating the algorithm until it reaches
$$
T_{n}
$$
, we obtain a sequence of trees 
$$
T^{0}\supseteq T^{1}\supseteq \cdots \supseteq T^{n}
$$
and a non-decreasing sequence of tuning parameters 
$$
\alpha_{0}\leq \alpha_{1}\leq \cdots \leq \alpha_{n}
$$
.

Based on the above result, optimal value 
$$
\alpha^{*}
$$
 can be obtained by cross validation:

- Initialization

  - For 
    $$
    v\in \left \{1,2,\cdots,V \right\}
    $$
    , set 
    $$
    D^{(v)}=D-D_{v}
    $$

  - Make a sequence
    $$
    \alpha_{1}',\alpha_{2}',\cdots,\alpha_{n-1}'
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

  - Make a sequence
    $$
    T^{(v,0)}\supseteq T^{(v,1)}\supseteq \cdots \supseteq T^{(v,n)}
    $$
    where 
    $$
    T^{(v,k)}
    $$
     is an optimal tree built from 
    $$
     D^{v}
    $$
     for a tuning parameter 
    $$
    {\alpha_{k}}'
    $$

- For 
  $$
  k=1,2,\cdots, n-1
  $$
  , compute cross validation error and choose 
  $$
  {\alpha_{k}}'
  $$
   that minimizes the error

<br>
You can see that the algorithm uses 
$$
{\alpha_{k}}'
$$
, a geometric mean of 
$$
\alpha_{k} 
$$
 and 
$$
 \alpha_{k+1}
$$
, to make computation simpler. Note that this is possible because 
$$
T(\alpha)=T(\alpha_{k})
$$
 for 
$$
 \alpha_{k} \leq \alpha < \alpha_{k+1}
$$
.

Decision tree we've discussed so far is called CART(Classification And Regression Tree), which has following characteristics:

- Robust to scale of features

- Robust to outliers

- Any data type can be used

- Easy to deal with categorical features; no need for dummification

- Easy to interpret

- Computation is fast

  - Only store data of terminal nodes

  - For classification, 
    $$
    O(DKN)
    $$
     complexity where
    
    $$
    \begin{equation}
      \begin{array}{l}
      D:the\; number\; of \; dimensions \\
      K:the\; number\; of\; classes \\
      N:the\; number\; of \; samples\;
      \end{array}
    \end{equation}
    $$

- Useful than linear regression, especially when non-linear relationship exists(regression)

- Prediction is relatively less accurate than other models(has high variance)
  
  - Due to hierarchical structure of the model; error of parent nodes are spread to the child nodes

- Discontinuities exist in split points(regression)


<br>
# CHAID

CHAID(Chi-Squared Automatic Interaction Detection) is an another way of building decision tree. Overall structure of CHAID is similar, but it differs from CART in that:

- it can split a node into more than 2 nodes
- it finds the optimal split by using test statistic 



For classification,

$$
(j^{*},s^{*})=\underset{(j,s)}{argmax}(\chi^{2})
$$

 where 
$$
\chi^{2}
$$
 is a chi-squared statistic to test homogeneity among different groups of feature. That is, the best pair of 
$$
j
$$
 and 
$$
s
$$
 is the pair which maximizes heterogeneity of distribution among different groups. Similarly, for regression, F-statistic from ANOVA is used to find the optimal value of 
$$
(j,s)
$$
. 




<br>
# Implementation using Python

## CART

For classification problem, we will use iris dataset: 

~~~python
from sklearn.datasets import load_iris

X, y = load_iris(return_X_y=True)
feature_names = load_iris().feature_names
class_names = load_iris().target_names
~~~

<br>
We can make a decision tree for classification using *DecisionTreeClassifier* in *sklearn*

~~~python
from sklearn.tree import DecisionTreeClassifier

clf = DecisionTreeClassifier(criterion='entropy', splitter='best', max_depth=5, min_samples_split=50, min_samples_leaf=50, 
                             random_state=0, max_leaf_nodes=10)
~~~

where 

- max_depth: maximum depth
- max_leaf_nodes: the maximum number of terminal nodes
- min_samples_leaf: the minimum number of samples that each terminal node should have
- min_samples_split: the minimum number of samples to be splitted


<br>
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


<br>
For evaulation of the model, we can use *cross_val_score*

~~~python
from sklearn.model_selection import cross_val_score

res = cross_val_score(clf, X, y, cv=5)
res.mean()
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
Also, we can check feature importance:

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

which returns value based on the (normalized) total reduction of the criterion brought by each feature.

<br>
Now, for hyperparameter optimization, let's start with grid search

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


<br>
Consequently, we can build the optimized tree as illustrated below.

![classification tree2]({{ site.urlimg }}/Decision-Tree/classification tree2.png)


<br>
We can also optimize hyperparameters by cost complexity pruning. First, obtain a sequence of 
$$
\alpha
$$
 with corresponding impurity
~~~python
cc_pruning = clf_full.cost_complexity_pruning_path(X, y)
cc_pruning
~~~

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
 based on the previous result:

~~~python
alphas = []
for i in range(len(cc_pruning['ccp_alphas'])-1):
    temp = np.sqrt(cc_pruning['ccp_alphas'][i]*cc_pruning['ccp_alphas'][i+1])
    alphas.append(temp)
~~~

<br>
Using the geometric sequence, we can obtain accuracy by cross validation:

|  Alpha   | Accuracy |
| :------: | :------: |
| 0.000000 | 0.953333 |
| 0.018366 | 0.953333 |
| 0.022665 | 0.953333 |
| 0.036161 | 0.960000 |
| 0.059897 | 0.946667 |
| 0.187907 | 0.933333 |
| 0.650011 | 0.666667 |

<br>
Thus, we can find 
$$
\alpha^{*}=0.650011
$$
and construct the pruned tree: 

![classification tree3]({{ site.urlimg }}/Decision-Tree/classification tree3.png)

<br>
The evaluation result of 3 difference cases can be summarized as:

|         Optimization Method          | Accuracy |
| :----------------------------------: | :------: |
|        random hyperparameter         |  0.660   |
|       optimized by grid search       |  0.767   |
| optimized by cost complexity pruning |  0.960   |


Although grid search increases accuracy, in many cases, optimized hyperparameters are at most local optimum. On the other hand, by cost complexity pruning, we can attain much higher level of accuracy by controlling tuning parameter 
$$
\alpha
$$
.

<br>
Similarly you can build a regression tree, using *DecisionTreeRegressor*:

~~~python
from sklearn.tree import DecisionTreeRegressor

reg = DecisionTreeRegressor(criterion='mse', splitter='best', max_depth=2, min_samples_split=50, min_samples_leaf=50, random_state=0, 
                            max_leaf_nodes=10)
~~~

![regression tree2]({{ site.urlimg }}/Decision-Tree/regression tree2.png)


<br>
Unlike classification tree, regression tree

- uses MSE as an criterion
- uses R-squared instead of accuracy
- splits a node by dividing into intervals

and remaining things can be done in a similar way(click [here](https://github.com/ykkim123/Data_Science/blob/master/Decision%20Tree.ipynb) for more detail).

<br>
## CHAID

For classification problem, we will use the following data.

~~~python
X = np.array(([1, 2, 3]*5) + ([2, 2, 3]*5)).reshape(10, 3)
y = np.array(([1]*5) + ([2]*5))
~~~


<br>
As explained, the algorithm finds the optimal split based on 
$$
\chi^{2}
$$
 statistic

~~~python
from CHAID import Tree

tree = Tree.from_numpy(X, y, split_titles=['a', 'b', 'c'], max_depth=2, min_parent_node_size=5, min_child_node_size=5, 
                       dep_variable_type='categorical') #hyperparameters are defined similarly
tree.print_tree()
~~~

which outputs

![CHAID]({{ site.urlimg }}/Decision-Tree/CHAID.png)


<br>
Similarly, you can apply CHAID to regression problem 

~~~python
np.random.seed(0)
y = np.random.normal(300, 100, 10)
~~~


<br>
by changing *dep_variable_type* to *categorical*:

~~~python
tree = Tree.from_numpy(X, y, split_titles=['a', 'b', 'c'], max_depth=2, min_parent_node_size=3, min_child_node_size=3,
                       dep_variable_type='continuous')
tree.print_tree()
~~~

![CHAID2]({{ site.urlimg }}/Decision-Tree/CHAID2.png)

<br>
## References

- [http://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote16.html](http://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote16.html)

- [http://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote17.html](http://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote17.html)

- [https://online.stat.psu.edu/stat508/lesson/11/11.8/11.8.2](https://online.stat.psu.edu/stat508/lesson/11/11.8/11.8.2)

- [https://github.com/Rambatino/CHAID](https://github.com/Rambatino/CHAID)
