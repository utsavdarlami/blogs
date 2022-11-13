+++
title = "Recurrent Neural Networks"
author = ["felladog"]
date = 2021-03-02T12:40:00+05:45
lastmod = 2021-09-13T12:29:00+05:45
tags = ["RNN", "Sequence Models"]
categories = ["Deep Learning"]
draft = false
+++

---

-   Acknowledgement
    -   <https://d2l.ai/chapter%5Frecurrent-neural-networks/index.html>
    -   <https://github.com/fastai/fastbook/blob/master/12%5Fnlp%5Fdive.ipynb>
    -   <https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-recurrent-neural-networks>
    -   <https://colah.github.io/posts/2015-08-Understanding-LSTMs/>  [Mainly LSTM]
    -   <https://deeplearning.cs.cmu.edu/S20/document/recitation/recitation-7.pdf>
    -   <http://cs231n.stanford.edu/slides/2017/cs231n%5F2017%5Flecture10.pdf>
    -   <https://www.youtube.com/watch?v=6niqTuYFZLQ>
    -   <https://github.com/fastai/fastbook/blob/master/12%5Fnlp%5Fdive.ipynb> [ fast ai nlp dive rnn archi]

    -   <http://ethen8181.github.io/machine-learning/deep%5Flearning/rnn/1%5Fpytorch%5Frnn.html#Recurrent-Neural-Network-(RNN)> [Main reference]
    -   <https://medium.com/ecovisioneth/building-deep-multi-layer-recurrent-neural-networks-with-star-cell-2f01acdb73a7> [Multi Layer]
    -   <https://towardsdatascience.com/pytorch-basics-how-to-train-your-neural-net-intro-to-rnn-cb6ebc594677>
    -   <https://www.jeremyjordan.me/introduction-to-recurrent-neural-networks/>
    -   <https://towardsdatascience.com/pytorch-basics-how-to-train-your-neural-net-intro-to-rnn-cb6ebc594677>
    -   [Sebastian Raschka Character Generation using lstm cell pytorch](https://github.com/rasbt/stat453-deep-learning-ss21/blob/main/L19/character-rnn/01%5Fchar-rnn%5Flstmcell-ver.ipynb)

---

-   Recurrent neural networks (RNNs) are designed to better handle sequential information. (text and stocks)

-   RNNs introduce state variables(hidden state) to store past information, together with the current inputs, to determine the current outputs.

{{< figure src="/ox-hugo/2021-06-03_18-21-51_screenshot.png" caption="Figure 1: rnn vs nn" width="420" height="600" target="/blogs" >}}

{{< figure src="/ox-hugo/rnn2.png" caption="Figure 2: a folded rnn" width="700" height="550" target="/blogs" >}}

{{< figure src="/ox-hugo/rnn.png" caption="Figure 3: a unfolded rnn" width="700" height="550" target="/blogs" >}}

-   Internal State of RNN

{{< figure src="/ox-hugo/my_internal.png" caption="Figure 4: a state diagram," width="700" height="550" target="/blogs" >}}


## Pytorch Implementation {#pytorch-implementation}


## Summary {#summary}