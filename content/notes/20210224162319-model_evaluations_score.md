+++
title = "ML Model Evaluation"
author = ["felladog"]
date = 2021-02-24T16:23:00+05:45
lastmod = 2021-07-08T09:01:25+05:45
tags = ["f1 score", "dice score", "precision", "recall"]
categories = ["Machine Learning"]
draft = false
+++

---

-   references
    -   To Read:
        -   <https://towardsdatascience.com/handling-imbalanced-datasets-in-machine-learning-7a0e84220f28>
-   questions

---

[ml]({{< relref "20210119123811-machine_learning" >}}) model evaluation methods


## Classfication {#classfication}


### Accuracy {#accuracy}


### Precision {#precision}

-   Precision = \\(\frac{True Positive}{True Positive + False Positive}\\)
-   From all the postive prediction given by our hypothesis/model how many examples where true positive


### Recall {#recall}

-   Recall = \\(\frac{True Positive}{True Positive + False Negative}\\)
-   From all the postive examples how many examples where correctly classified by our hypothesis/model


### F1 Score {#f1-score}

-   A harmonic mean between recall and precision
    -   Why ?
        -   Tries to give the lowest value between recall and precision
        -   biased to the lowest value
        -   Balances recall and precision
-   F1 Score = \\(\frac{2}{\frac{1}{Precision} + \frac{1}{Recall}}\\)

-   From Wikipedia
    -   _In information retrieval and machine learning, the harmonic mean of the precision and the recall is often used as an aggregated performance score for the evaluation of algorithms and systems: the F-score (or F-measure). This is used in information retrieval because only the positive class is of relevance, while number of negatives, in general, is large and unknown.[14] It is thus a trade-off as to whether the correct positive predictions should be measured in relation to the number of predicted positives or the number of real positives, so it is measured versus a putative number of positives that is an arithmetic mean of the two possible denominators._


### Dice Score {#dice-score}

-   It is F1 Score
-   Dice Score = \\(\frac{2 \* Intersection}{Union + Intersection}\\)

    = \\(\frac{2\*TP}{2\*TP + FP + FN}\\)
-   For Image Segmentaion evaluation


### Confusion Matrix {#confusion-matrix}

-   The scikit learn confusion matrix representation will be a bit different, as scikit learn considers
    -   the actual target classes as columns
    -   the predicted classes as rows,

{{< figure src="/ox-hugo/cf_matrix.png" width="450" height="250" target="/blogs" >}}
