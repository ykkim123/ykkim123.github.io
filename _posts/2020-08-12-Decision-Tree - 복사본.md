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


## Decision Tree for Regression

Decision tree for regression has similar structure to that of classification. However, it computes prediction by <br>

Then, the model would look like below:

![regression tree]({{ site.urlimg }}/Decision-Tree/regression tree.png)



Then, the algorithm finds the optimal split that satisfies <br>
$$
(j^{*},s^{*})=\underset{(j,s)}{argmin}(\sum_{x_{i}\in R_{1}(j,s)}(y_{i}-\bar{y}_{1})^{2}+\sum_{x_{i}\in R_{2}(j,s)}(y_{i}-\bar{y}_{2})^{2})
$$
where <br>
$$
\begin{equation}
  \begin{array}{l}
  R_{1}(j,s)=\left\{{X|X_{j}\leq s} \right\} \\
  R_{2}(j,s)=\left\{{X|X_{j}>s} \right\}
  \end{array}
\end{equation}
$$



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

