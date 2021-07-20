+++
title = "activation function"
author = ["felladog"]
date = 2021-05-22T16:27:00+05:45
lastmod = 2021-07-13T20:53:41+05:45
tags = ["no_tags"]
categories = ["uncategorized"]
draft = false
+++

---

-   References :
    -   Reading:
        -   Tom Mitchell Lectures slides of chapter 4
        -   Tom Mitchell, Machine Learning Chapter 4
-   Questions :

---

-   Also known as squashing Function

{{< figure src="/ox-hugo/2021-06-03_15-42-52_screenshot.png" caption="Figure 1: Activation Functions, from this [post](https://zzsza.github.io/data/2018/05/13/cs231n-backpropagation-and-neural-networks/)" width="550" height="310" target="/blogs" >}}


## Sigmoid {#sigmoid}

-   output is a non linear function of its inputs and is differentiable threshold function
-   also called logistic function
-   output ranges from 0 to 1

{{< figure src="/ox-hugo/2021-06-03_15-35-46_screenshot.png" caption="Figure 2: The sigmoid threshold unit, from Tom Mitchell Lectures" width="450" height="300" target="/blogs" >}}


## Tanh {#tanh}

-   \\(f(x) = tanh(x) = \frac{(e^{x} - e^{-x})} {(e^{x} + e^{-x})}\\)

-   \\(f'(x) = 1 - (tanh(x))^{2}\\)


## ReLU {#relu}