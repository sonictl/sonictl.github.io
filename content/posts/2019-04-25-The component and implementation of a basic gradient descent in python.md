---
layout: post
title: 'The component and implementation of a basic gradient descent in python'
date: 2019-04-25 13:02:00 +0800
category: from_cnblogs
slug: p20190425130200
---
in my impression, the gradient descent is for finding the independent variable that can get the minimum/maximum value of an objective function. So we need an obj. function: $\mathcal{L}$

- an obj. function: $\mathcal{L}$
- The gradient of $\mathcal{L}: 2x+2$
- $\Delta x$ , The value of idependent variable needs to be updated: $x \leftarrow x+\Delta x$
- 

### 1. the $\mathcal{L}$ is a context function: $f(x)=x^2+2x+1$

how to find the $x_0$ that makes the $f(x)$ has the minimum value, via gradient descent?

Start with an arbitrary $x$, calculate the value of $f(x)$ :

```python
import random
def func(x):
  return  x*x + 2*x +1
def gred(x):   # the gradient of f(x)
  return 2*x + 2

x = random.uniform(-10.0,10.0)  #randomly pick a float in interval of (-10, 10)
# x = 10
print('x starts at:', x)

y0 = func(x) #first cal
delta = 0.5  #the value of delta_x, each iteration
x = x + delta

# === interation ===
for i in range(100):
  print('i=',i)
  y1 = func(x)
  delta = -0.08*gred(x)
  print('  delta=',delta)
  if y1 > y0:
    print('  y1>y0')
    # if gred(x) is positive, the x should decrease.
    # if gred(x) is negative, the x should increase.
  else:
    print('  y1<=y0')
    # if gred(x) is positive, the x should increase.
    # if gred(x) is negative, the x should decrease.
  x = x+delta
  y0 = y1
  print('  x=', x, 'f(x)=', y1)
```


Let's disscuss how to determin the `some_value` in the psudo code above.

if $y_1-y_0$ has a large positive difference, i.e. $y1 >> y0$, the x should shift backward heavily. so the `some_value` can be a ratio of $(y_1-y_0)\times(-gradient)$ , Let's say, `some_value`:  $\lambda = r \times$ gred(x) , here, $r=0.08$ is the step-size.

The basic gradient descent has many shortcomings which can be found by search the 'shortcoming of gd'.

Another problem of GD algorithm is , **What if the $\mathcal{L}$ does not have explicit expression of its gradient?**

Stochastic Gradient Descent(SGD) is another GD algorithm.