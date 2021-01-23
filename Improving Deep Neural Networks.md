## Set up
### Train test split

In normal machine learning projects, one usually has a 80:20 or 70:30 train:test split, or a 60:20:20 train:dev:test split.
In deep learning, we often are using tens of thousands or even millions of datapoints. So it's ok to use a 98:1:1 split if that equates to 980,000:10,000:10,000 for example.

### Bias and variance

You have a **high bias problem** if your **training accuracy** is low. This indicates that your model is underfitting the data. In this case you could

* Train longer
* Bigger network
* NN architectures

You have a **high variance problem** if your **training accuracy** is high but your **testing accuracy** is low. This indicated overfitting and it is a very common problem. In this case you should try:

* Get more data
* Regularize your model
* NN architectures

In both these examples the optimal error or **Bayes error** is what we should be comparing the training error against. This is the theoretical best or human level error possible. In a cat classifier example this would be almost 0%, but in something like a cat species classifier this could be much more.

Basic Recipe:

1. High bias problem? Ok, fix that first.
2. High variance problem? All right, now fix that. Then go back to step 1 and repeat.

## Regularisation

### L2 regularisation

Add term to loss function

$$
J(\theta) = \frac{1}{m} \sum_{i=1}^{m} L(\hat{y^{(i)}}, y^{(i)}) + \frac{\lambda}{2m} \sum_l \left \lvert \lvert W^{[l]} \right \rvert \rvert_F^2 \quad \text{where} \quad
\left \lvert \lvert W \right \rvert \rvert_F^2 = \sum_{i,j} W_{i,j}^{2}
$$

### Dropout

Randomly delete nodes in the network.


## Setting up the problem

### Input normalisation

Inputs should be normalised. it leads to better performance.

### Weight initialisation

To avoid the problem of vanishing/exploding gradient, carefully selected initial starting weights greatly improve the speed of learning. 

### Grad check

Numerically check your implementation of backpropogation is correct.

## Optimisation algorithms

Given that we have so much data, fast optimization algorithms are cricual.

### Mini-batch gradient descent

Split training data into "mini-batches".

We have training data $X = [x^{(1)} \ x^{(2)} \ \ldots \ x^{(m)}]$, an $n_x \times m$ matrix where we have stacked each training example into columns. The corresponsing labels are represented by the $1 \times m$ matrix $Y = [y^{(1)} \ y^{(2)} \ \ldots \ y^{(m)}]$. In a vectorised implementation of gradient descent, all $m$examples will be fed through the network and used in backpropogation to issue a single update to the model weights. In cases where the number of training examples is huge, this single step will take a long time to compute. 

In mini-batch gradient descent, we split our training data into "batches" of a set size. Let $m=5,000,000$ and let our batch size be 1000. Then to form mini-batches we take partitions $X = X^{\{1\}} \cup X^{\{2\}} \cup \ldots \cup X^{\{5000\}}$  and $Y = Y^{\{1\}} \cup Y^{\{2\}} \cup \ldots \cup Y^{\{5000\}}$. Then we perform gradient descent on each mini-batch as normal, and loop over all mini-batches.

A mini-batch size of 1 is called stochastic gradient descent. SGD, or any batch-size, in not guarenteed to decrease the overall cost function $J$ every iteration. Instead, the cost function appears to move more randomly, but in the general direction of the minimum. The cost function will not actually hit the minimum but will oscillate around. This oscillation can be controlled by lowering the learning rate.

In practice the fastest algorithm will have a batch size somewhere in the middle, to make use of vectorisation. Batch sizes of 64, 128, 256 and 512 are common.

### Gradient Descent with Momentum

Idea: each iteration of gradient descent, you average the gradients so as to obtain a smoother path.

$$
V_{dW} = \beta V_{dW} + (1- \beta) dW \\
V_{db} = \beta V_{db} + (1- \beta) db \\
\\
W = W - \alpha V_{dW} \\
b = b - \alpha V_{db} \\
$$

This almost always works better than without momentum.

### RMSprop

On each iteration t compute $dW, db$ on the current minibatch.

### Weight decay

Slowly reduce your learning rate over time. Here are some forumals people use.

$$
\alpha = \frac{1}{1+\text{decay\_rate} * \text{epoch\_num}} * \alpha_0
$$

