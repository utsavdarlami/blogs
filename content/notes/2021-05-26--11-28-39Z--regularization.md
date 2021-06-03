+++
title = "regularization"
author = ["felladog"]
date = 2021-05-26T17:13:00+05:45
lastmod = 2021-06-03T20:14:35+05:45
tags = ["no_tags"]
categories = ["machine learning"]
draft = false
+++

---

-   References :
    -   To read:
        -   <https://www.wikiwand.com/en/Regularization%5F(mathematics)>
        -   <https://www.wikiwand.com/en/Early%5Fstopping>

-   Questions :

---

In machine learning and regularization is the process of adding information in order to solve an ill-posed problem or to prevent overfitting.


## Early Stopping {#early-stopping}

-   Form of regularization used to avoid overfitting when training a learner with an iterative method, such as gradient descent.
-   Such methods update the learner so as to make it better fit the training data with each iteration. Up to a point, this improves the learner's performance on validation set. Past that point, however, improving the learner's fit to the training data comes at the expense of increased generalization error.
-   It provides guidance as to how many iterations can be run before the learner begins to over-fit.
-   Is implemented using one data set for training, one statistically independent data set for validation and another for testing. The model is trained until performance on the validation set no longer improves and then applied to the test set.


## For Deep learning {#for-deep-learning}

-   Dropout
-   [Batch Normalization]({{< relref "2021-03-06--02-21-33Z--batch_normalization" >}})
