+++
title = "Batch Normalization"
author = ["felladog"]
date = 2021-03-06T08:06:00+05:45
lastmod = 2021-05-01T11:49:57+05:45
tags = ["BatchNorm", "Normalization"]
categories = ["deep learning"]
draft = false
+++

---

-   Acknowledgement :
    -   [d2l.ai batch normalization](https://d2l.ai/chapter%5Fconvolutional-modern/batch-norm.html)
    -   [L11.2 How BatchNorm Works - Sebastian Raschka](https://www.youtube.com/watch?v=34PDIFvvESc&list=PLTKMiZHVd%5F2KJtIXOW0zFhFfBaJJilH51&index=83)
    -   <https://twitter.com/karpathy/status/1013245864570073090>

-   Questions :
    1.  What is Normalization?
        -   [Normalization]({{< relref "2021-03-06--02-35-46Z--normalization" >}})

---

-   A popular and effective technique that constantly accelerates the convergence of deep network.
-   Normalizes hidden layer inputs.
-   Helps with exploding/vanishing gradient problems.
-   Can increase the training stability and convergence rate.
-   Can be understood as additional(normalization) layers(with additional parameters).


## How is it done {#how-is-it-done}

-   Batch normalization is applied to individual layers (optionally, to all of them) and works as follows:
    -   In each training iteration, we first normalize the inputs (of batch normalization) by subtracting their mean and dividing by their standard deviation, where both are estimated based on the statistics of the current minibatch.
    -   Next, we apply a scale coefficient and a scale offset. It is precisely due to this normalization based on batch statistics that batch normalization derives its name.

-   Formulas :

    -   \\(BN(x) = \gamma \odot \frac{x - \hat{\mu}\_{B}}{\hat{\sigma}\_{B}} + {\beta}\\)

    -   where,
        -   \\(\hat{\mu}\_{B}\\) is the sample mean,
            -   \\(\hat{\mu}\_{B}  = \frac{1}{|B|} \displaystyle\sum\_{x\in{B}} {x}\\)
        -   \\(\hat{\sigma}\_{B}\\) is the sample standard deviation of the minibatch B
            -   \\(\hat{\sigma}\_{B} = \frac{1}{|B|} \displaystyle\sum\_{x\in{B}} {(x - \hat{\mu}\_{B})^2} + \epsilon\\)
            -   \\(\epsilon\\) is for numerical stability
        -   \\(\gamma\\) is scale parameter, \\(\beta\\) is shift parameter, elementwise and have same shape as \\(x\\), they need to be learned jointly with the other model parameters

    <!--listend-->

    -   \\(\gamma\\) controls the spread or scales
    -   \\(\beta\\) controls the mean,
        -   It also makes the bias unit redundant so we can remove the bias from previous layers.
    -   The network can learn \\(\gamma\\) = \\(\hat{\sigma}\_{B}\\) and \\(\beta\\) = \\(\hat{\mu}\_{B}\\) undoing the normalization part


## BatchNorm in Pytorch {#batchnorm-in-pytorch}

```python
batch_norm = torch.nn.BatchNorm2d(num_features, # no of channels
				  eps=1e-05, # the value used for the running_mean and running_var computation.
				  momentum=0.1,
				  affine=True,  # With Learnable Parameters
				  track_running_stats=True)
```

```python
import torch
from torch import nn

m = nn.BatchNorm2d(64)  # C
# Input : (N, C, H, W)
in_ = torch.rand(32, 64, 68, 68)
# Output : (N, C, H, W) (same shape as input)
out_ = m(in_)
print("Results : ")
print(out_.shape) # torch.Size([32, 64, 68, 68])
print(f"Mean =  {out_.mean().item()}")  # ~ 0
print(f"SD  = {out_.std().item()}")   # ~ 1
```

```text
Results :
torch.Size([32, 64, 68, 68])
Mean =  -4.679170828580936e-09
SD  = 0.999940037727356
```


### Implementation Notes {#implementation-notes}

-   BatchNorm1d(num\_of\_units\_in\_prev\_layer) for Linear Layers.
-   BatchNorm2d(prev\_out\_channels) for Conv Layers.
-   BatchNorm can be used before or **after** the activation layer but it was initial proposed to be used before activation layer. If using Dropout, keep it after the activation layer.
-   _Use bias=False for your Linear/Conv2d layer when using BatchNorm, or conversely forget to include it for the output layer. - Andrej Karpathy_.

    {{<tweet 1013245864570073090>}}

-   Toggle model.train() and model.eval() for inference.
-   Stable with larger minibatch size > (16, 32, 64)


## BatchNorm during prediction("Inference") {#batchnorm-during-prediction--inference}

1.  [ **Preferred Way** ] Using exponentially weighted avg(moving avg) of mean and variance
    -   running\_mean = momentum \* running\_mean + (1 - momentum) \* sample\_mean, where,
        -   running\_mean is the mean from the previous batch
        -   sample mean is the mean from the current batch
        -   momentum is choosed as 0.1
    -   Similarly for variance we can calculate running\_variance
    -   This running mean and variance will be used during the inference for scaling datapoints.
2.  Alternatively can also use global training set mean and variacnce


## Why batch normalization works? {#why-batch-normalization-works}

-   The first paper on batch norm presented that it reduces the internal covariate shift _(It means that the layers distribution changes)_ but no strong evidence is available.
-   One another theory could be that it just provides additional parameters helping in decoupling the layers(make the layers independent). Let's say if one layers screws up the other next layers can be robust if batch norm is applied.
-   Paper [How does batch normalization help optimization?](https://arxiv.org/abs/1805.11604) shows
    -   BatchNorm makes the optimization landscape significantly smoother. This smoothness induces a more predictive and stable behavior of the gradients, allowing for faster training.
    -   The BatchNorm enables faster convergence by allowing larger learning rate.
    -   Good performance seems unrelated to covariate shift reduction.


## Summary {#summary}

-   During model training, batch normalization continuously adjusts the intermediate output of the neural network by utilizing the mean and standard deviation of the minibatch, so that the values of the intermediate output in each layer throughout the neural network are more stable.

-   The batch normalization methods for fully-connected layers and convolutional layers are slightly.

-   Like a dropout layer, batch normalization layers have different computation results in training mode and prediction mode.

-   Batch normalization has many beneficial side effects, primarily that of regularization. On the other hand, the original motivation of reducing internal covariate shift seems not to be a valid explanation.
