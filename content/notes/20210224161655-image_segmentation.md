+++
title = "image segmentation"
author = ["felladog"]
date = 2021-02-24T16:16:00+05:45
lastmod = 2021-07-13T09:50:38+05:45
tags = ["image"]
categories = ["Machine Learning"]
draft = true
+++

---

-   referencees :
    -   <https://www.youtube.com/watch?v=AZr64OxshLo>
    -   <https://www.youtube.com/watch?v=0FmNxqLFeYo>
    -   <https://stats.stackexchange.com/questions/273537/f1-dice-score-vs-iou>
-   questions :

---


## Using Classical Approach {#using-classical-approach}


### [Histogram Based Segmentation]({{< relref "2021-07-04--16-24-59Z--histogram_based_segmentation" >}}) {#histogram-based-segmentation--2021-07-04-16-24-59z-histogram-based-segmentation-dot-md}


### [thresholding]({{< relref "2021-05-27--05-20-06Z--thresholding" >}}) {#thresholding--2021-05-27-05-20-06z-thresholding-dot-md}


### Random Walker Segmentation {#random-walker-segmentation}


### Watershed Segmentation {#watershed-segmentation}


### Graph based segmentation {#graph-based-segmentation}

-   </home/felladog/Downloads/CV_Nptel/W2/DL4CV_Week02_Part05.pdf>
-   <https://youtu.be/0HbRnFTOOms?t=680>


## Using Unsupervised Approach {#using-unsupervised-approach}


### [Gaussian Mixture Model]({{< relref "2021-04-13--13-20-35Z--gaussian_mixture_model" >}}) {#gaussian-mixture-model--2021-04-13-13-20-35z-gaussian-mixture-model-dot-md}


### [Kmeans Clustering]({{< relref "2021-05-23--03-19-46Z--kmeans_clustering" >}}) {#kmeans-clustering--2021-05-23-03-19-46z-kmeans-clustering-dot-md}


## Using Deep Learning {#using-deep-learning}


### [UNET]({{< relref "2021-05-26--11-48-24Z--unet" >}}) {#unet--2021-05-26-11-48-24z-unet-dot-md}


## Image Segmentation Loss/Evaluation Method {#image-segmentation-loss-evaluation-method}

-   Problem with other common metrics
-   We need metrics that target pixels in foreground


### Intersection over union (Jaccard index) {#intersection-over-union--jaccard-index}

-   [sklearn jaccard module](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.jaccard%5Fscore.html)
-   Overall accuracy is not a good metric for semantic segmentation, especially for imbalanced dataset
-   IOU = \\(\frac{Intersection}{Union}\\)

    = \\(\frac{\sum{\hat{y}\*y}} {\sum{\hat{y} + y - \hat{y} \* y}}\\)

    = \\(\frac{TP}{TP + FP + FN}\\)


### [Dice Score]({{< relref "20210224162319-model_evaluations_score" >}}) {#dice-score--20210224162319-model-evaluations-score-dot-md}

-   Easy to differentiate
-   For Image = \\(\frac{2\*\sum{\hat{y}\*y}} {\sum{\hat{y} + y}}\\)
-   priortise both precision and recall equally
-   Loss = 1 - Dice


### IOU vs Dice Score {#iou-vs-dice-score}

_IoU metric tends to penalize single instances of bad classification more than the F1/Dice score quantitatively even when they can both agree that this one instance is bad._
