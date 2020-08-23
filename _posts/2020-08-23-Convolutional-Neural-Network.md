---
layout: post
title: "Convolutional Neural Network"
date: "2020-08-12"
description: "This post explains how to implement convolutional neural network using PyTorch; creating some basic structures of the algorithm, and actual implementations of it using MNIST dataset"
category: 
  - featured
# tags will also be used as html meta keywords.
tags:
  - Image Recognition
show_meta: true
comments: true
mathjax: true
gistembed: true
published: true
noindex: false
nofollow: false
---

First, run the code below so that we can use GPU for computation.

<code data-gist-id="5da556a66317060418af51fdab23fcd8" data-gist-file="Convolutional Neural Network.ipynb" data-gist-line="20-30"></code>

<br>

# Load data

Before loading MNIST dataset, define the *transformation* that converts data into PyTorch tensor and normalizes it by mean and standard deviation. Without normalization, cases where gradients do not converge may occur.

CODE

Also, you can normalize in either way:

- $$
  x_{i,new}=\frac{x_{i}-\mu_{i}}{\sigma_{i}}
  $$

  <br>

  where 
  
  <br>
  $$
  \begin{equation}
    \begin{array}{l}
      \mu_{i}=mean\; of\; ith\; feature\\
      \sigma_{i}=standard\; deviation\; of\; ith\; feature
  \end{array}
  \end{equation}
  $$
  
- $$
  x_{new}=\frac{x}{255}
  $$

  , since each pixel has value between 0 and 255

<br>

That is, in order to improve the algorithm, 

- Data should lie between 0 and 1
- Each feature should have the same range

<br>

Also, note that we normalized data by 
$$
((0.1307,),(0.3081,))
$$
, because MNIST dataset contains only black-and-white images; for colored images, mean and variance vector of length 3 respectively would be used.

Now, we can download MNIST dataset

CODE

<br>

and convert the images into data loaders 

CODE

where 

- batch size: size of data used for each iteration
- shuffle: whether to shuffle data every time data loader is called

<br>

For better understanding of the dataset, let's visualize the image. First, define *plot_img* that is implemented by the following procedure:

CODE

1. Convert tensor to numpy, then choose the first element

   - second element contains labels of each image

2. Denormalize data

   - from 
     $$
     x_{i,new}
     $$
     , extract 
     $$
     x_{i}
     $$

3. Plot the image

   - *cmap* option is used to map scalar data to colors

<br>

Then, we can visualize an image as below:

CODE

![digit visualization]({{ site.urlimg }}/Convolution Neural Network/digit visualization.png)

<br>

# Build Convolutional Neural Network

Below is the architecture of CNN.

CODE

Let's start from identifying what do some variables mean:

- self.conv1 = nn.Conv2d(1, 10, kernel_size=5, stride=1, padding=0)

  - 1 input channel and 10 output channels

    - 1 input channel because there exist only black-and-white images
    - 10 output channels correspond to applying 10 different filters

  - filter of size 
    $$
    5\times 5
    $$
     with stride=1 and padding=0

<br>

- self.conv2_drop = nn.Dropout2d()

  - dropout with default 
    $$
    p=0.5
    $$

<br>

- self.fc1 = nn.Linear(320,50)
  - takes 320 input features and 50 output features

<br>

Then, the whole procedure can be summarized as:

1. convolutional layer1- max pooling - ReLU activation
2. convolutional layer2- dropout - max pooling - ReLU activation
3. view
4. linear layer1 - ReLU activation
5. dropout
6. linear layer2
7. softmax

<br>

Note that *view(-1,320)* is used to convert shape of a tensor from 
$$
(32\times 20\times 4\times 4)
$$
to 
$$
(32\times 320)
$$
 so that it can be taken as an input of *nn.Linear(320,50)*.



<br>

Next, define *fit* for fitting the algorithm:

CODE

1. For training, set as *model.train()*. Otherwise, set as *model.eval()* and *volatile=True*

   - *volatile=True* implies that you won't be running backpropagation; this conserves memory

2. Initialize *running_loss* and *running_correct* for computation of loss and accuracy

3. For each batch, do:

   1. If you can use GPU, convert data to GPU tensor and assign them as variables
   2. For training, set as *optimizer.zero_grad()* to avoid accumulation of gradients
   3. After forward propagation, compute loss based on negative log likelihood
   4. Update *running_loss* by adding average loss of new batch
   5. Predict label as the one that has the highest probability
      - *dim=1* implies that we will find the maximum for each image
      - *[1]* under *keepdim=True* implies the we will find the label which has the highest probability(largest output)
   6. Update *running_correct* by adding the total number of correctly predicted cases of new batch
   7. Under training, calculate the gradient of loss function and update parameters

4. Loss and accuracy of the whole dataset are computed as 
   $$
   \begin{equation}
     \begin{array}{l}
       loss=\frac{\sum negative\; log\; likelihood}{N} \\
       accuracy=100\times \frac{total\; number\; of\; correctly\; predicted\; cases}{N}
     \end{array}
   \end{equation}
   $$
   <br>

   where 
   $$
   N
   $$
    is size of dataset.

<br>

Now, we can train and evaluate the algorithm iteratively 

CODE

<br>

and visualize the result as below.

![loss]({{ site.urlimg }}/Convolution Neural Network/loss.png)

![accuracy]({{ site.urlimg }}/Convolution Neural Network/accuracy.png)

<br>

Some other applications of the algorithm can be found here. 

<br>

# References

- [https://github.com/svishnu88/DLwithPyTorch](https://github.com/svishnu88/DLwithPyTorch)
