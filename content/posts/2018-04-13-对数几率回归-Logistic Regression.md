---
layout: post
title: '对数几率回归-Logistic Regression'
date: 2018-04-13 12:12:00 +0800
category: from_cnblogs
slug: p20180413121200
---
Finally, I'd use English for excercise and for conveniently type in the equations without switch.
Table of content:

[TOC]

# Recall, Implement and Experiment for Logistic Regression
## 1. Recall the Linear Model
Linear regression is for learing the parameter of $\boldsymbol{w}$ and $\boldsymbol{b}$ for:
    $$ f(x_i) = wx_i + b,\,\,\, \mathrm{and \,\, let \,\,} f(x_i) \simeq y_i$$


## 2. Logistic regression
Logistic regression is a linear model for classfy data.
　　The generalized linear model equation:

　　$$ y = g^{-1} (\boldsymbol{w}^T\boldsymbol{x} +b) $$
the $g(\cdot)$ is called the **link function**. So, the log-linear regression is the one when $g(\cdot) = \mathrm{ln}(\cdot)$ .
ok, Let's disscuss the Log-probability Regression.
##Logistic Regression
Logistic regression is for classifying, consider that the $y $ has only two value: 0 and 1, i.e. $y \in \{0,1\}$.
use the mapping $y = \frac {1}{ 1+e^{-z}}$ to map the real value of $z = \boldsymbol{w}^T\boldsymbol{x} + b$ into the value in the interval of (0,1).
So, the derivation: $$ \mathrm{ln} \frac{y}{1-y} = \boldsymbol{w}^T\boldsymbol{x} +b$$
the fraction: $ \frac{y}{1-y}$ can be treat as the odd = $ P(\mathrm{positive}) / P(\mathrm{Negative})$.
So we write it as below:
$$ \mathrm{ln}  \frac{p(y=1|\boldsymbol{x})}{p(y=0|\boldsymbol{x})} = \boldsymbol{w}^T\boldsymbol{x} +b$$
and we can get $$p(y=1|\boldsymbol{x}) = \frac{e^{\boldsymbol{w}^T\boldsymbol{x} +b}}{1+e^{\boldsymbol{w}^T\boldsymbol{x} +b}}$$ 
and 
$$p(y=0|\boldsymbol{x}) = \frac{1}{1+e^{\boldsymbol{w}^T\boldsymbol{x} +b}}$$
### The model:
to maximize the likelihood:
    $$\ell(\boldsymbol{w},b) = \sum_{i=1}^{m} \mathrm {ln} p(y_i|$$