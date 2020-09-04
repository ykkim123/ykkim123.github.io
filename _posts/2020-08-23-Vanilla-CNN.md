---
layout: post
title: "Vanilla CNN"
date: "2020-08-12"
description: "This post explains how to implement vanilla convolutional neural network using PyTorch-from building basic structures of the algorithm to actual implementation using MNIST dataset."
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

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="23-25"></code>

<br>

# Load data

Before loading MNIST dataset, define *transformation* which converts data into PyTorch tensor and normalizes it by mean and standard deviation. Normalizing data is important, since gradients may not converge otherwise.

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="33"></code>

You can normalize data in either way:

- $$
  x_{i,new}=\frac{x_{i}-\mu_{i}}{\sigma_{i}}
  $$

  where 
  $$
  \mu_{i}
  $$
  and 
  $$
  \sigma_{i}
  $$
   are the mean and standard deviation of ith feature

<br>

- $$
  x_{new}=\frac{x}{255}
  $$

  because each pixel has value between 0 and 255

<br>

That is, to improve the algorithm, 

- Data should lie between 0 and 1
- Each feature should have the same range

<br>

Note that we normalized data by 0.1307 and 0.3081, because MNIST dataset contains only black-and-white images; for colored ones, mean and standard deviation vector of length=3 should be used.

Now, we can download MNIST dataset and convert images into data loaders

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="39-40, 45-47"></code>

where 

- batch size: size of data used for every iteration
- shuffle: whether to shuffle data every time data loader is called

<br>

Also, we can visualize the image as below:

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad " data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="53-57,68-69,90"></code>

![digit]({{ site.urlimg }}/Vanilla_CNN/digit.png)

<br>

# Build vanilla CNN

Below is the basic structure of CNN.

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="171-187"></code>

Let's start from identifying variables:

- self.conv1 = nn.Conv2d(1,10,kernel_size=5,stride=1,padding=0)

  - 1 input channel and 10 output channels

    - 1 input channel because there exist only black-and-white images
    - 10 output channels correspond to applying 10 different filters

  - filter of size 
    $$
    5\times 5
    $$
     with stride=1 and padding=0

- self.conv2_drop = nn.Dropout2d()

  - dropout with default 
    $$
    p=0.5
    $$

- self.fc1 = nn.Linear(320,50)
  
  - takes 320 input features and 50 output features

<br>

Then, overall structure of the algorithm can be shown as

1. convolutional layer1- max pooling - ReLU activation
2. convolutional layer2- dropout - max pooling - ReLU activation
3. view
4. linear layer1 - ReLU activation
5. dropout
6. linear layer2
7. softmax



Note that 

- *view(-1,320)* is used to convert the shape of tensor from (32 X 20 X 4 X 4) to (32 X 320) so that it can be taken as an input of *nn.Linear(320,50)*

- the number of input channels in *nn.Linear(320,50)* corresponds to:
  1. 28 X 28: original data
  2. 24 X 24: after convolutional layer1 with filter size=5
  3. 12 X 12: after max pooling with stride=2
  4. 8 X 8: after convolutional layer2 with filter size=5
  5. 4 X 4: after max pooling with stride=2
  6. 20X 4 X 4 =320 where 20 is the number of output channels of *conv2*

<br>

Next, define a function for fitting the algorithm:

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="193-221"></code>

1. When phase=training, set as *model.train()*. Otherwise, set as *model.eval()* and *volatile=True*.

2. Initialize *running_loss* and *running_correct* for the computation of loss and accuracy.

3. For each batch, do:

   1. If you can use GPU, convert data to GPU tensor and make them variables.
   2. When phase=training, set as *optimizer.zero_grad()* to avoid accumulation of gradients.
   3. After forward propagation, compute loss based on negative log-likelihood.
   4. Update *running_loss* by adding average loss of new batch.
   5. Predict label as the one that has the highest probability.
   6. Update *running_correct* by adding the total number of correctly predicted cases of new batch.
   7. When phase=training, compute the gradient of loss function and update parameters.

4. Loss and accuracy of the whole dataset are computed as:
   
   <br>
   $$
   \begin{equation}
     \begin{array}{l}
        loss=\frac{\sum negative\; log-likelihood}{N} \\
        accuracy=100\times \frac{total\; number\; of\; correctly\; predicted\; cases}{N}
     \end{array}
   \end{equation}
   $$
   <br>
   
   where 
   $$
   N
   $$
    is the size of data.

<br>

Finally, we can train and evaluate the algorithm iteratively 

<code data-gist-id="f45f939f1c29fb2fcbf6144452381cad" data-gist-file="Vanilla_CNN-MNIST.py" data-gist-line="252-254,259-270"></code>

and visualize the result as below.

![loss]({{ site.urlimg }}/Vanilla_CNN/loss.png)

![accuracy]({{ site.urlimg }}/Vanilla_CNN/accuracy.png)

<br>

Also, some other applications of CNN can be found [here](https://github.com/ykkim123/Data_Science/tree/master/Vanilla_CNN).

<br>

# References

- [https://github.com/svishnu88/DLwithPyTorch](https://github.com/svishnu88/DLwithPyTorch)