+++
title = "ML Evaluations Score"
author = ["felladog"]
date = 2021-02-24T16:23:00+05:45
tags = ["f1 score", "dice score", "precision", "recall"]
draft = false
+++

---

-   references
-   questions

---


## Accuracy {#accuracy}


## Precision {#precision}

-   Precision = \\(\frac{True Positive}{True Positive + False Positive}\\)


## Recall {#recall}

-   Recall = \\(\frac{True Positive}{True Positive + False Negative}\\)


## F1 Score {#f1-score}

-   A harmonic mean between recall and precision
    -   Why ?
        -   Tries to give the lowest value between recall and precision
        -   biased to the lowest value
        -   Balances recall and precision
-   F1 Score = \\(\frac{2}{\frac{1}{Precision} + \frac{1}{Recall}}\\)

-   From Wikipedia
    -   _In information retrieval and machine learning, the harmonic mean of the precision and the recall is often used as an aggregated performance score for the evaluation of algorithms and systems: the F-score (or F-measure). This is used in information retrieval because only the positive class is of relevance, while number of negatives, in general, is large and unknown.[14] It is thus a trade-off as to whether the correct positive predictions should be measured in relation to the number of predicted positives or the number of real positives, so it is measured versus a putative number of positives that is an arithmetic mean of the two possible denominators._


## Dice Score {#dice-score}

-   It is F1 Score
-   Dice Score = \\(\frac{2 \* Intersection}{Union + Intersection}\\)
