---
layout: post
title: 'kwargs - Key words arguments in python function'
date: 2019-09-08 07:49:00 +0800
category: from_cnblogs
slug: p20190908074900
---
## This is a tutorial of how to use `*args` and `**kwargs`

For defining the **default value** of arguments that is not assigned in key words when calling the function:

```python
def func(**keywargs):
    if 'my_word' not in keywargs:
        word = 'default_msg'
        print(word)
    else:
        word = keywargs['my_word']
        print(word)
```
call this by:
```python
func()
func(my_word='love')
```
you'll get:
```
default_msg
love
```
read more about *args and **kwargs in python: https://www.digitalocean.com/community/tutorials/how-to-use-args-and-kwargs-in-python-3

**Add more about default value if arg key is not assigned:**
```python
def make_hastie_10_2(n_samples=12000, random_state=None):
    """Generates data for binary classification used in
    Hastie et al. 2009, Example 10.2.

    The ten features are standard independent Gaussian and
    the target ``y`` is defined by::

      y[i] = 1 if np.sum(X[i] ** 2) > 9.34 else -1

    Read more in the :ref:`User Guide <sample_generators>`.

    Parameters
    ----------
    n_samples : int, optional (default=12000)
        The number of samples.

    random_state : int, RandomState instance or None (default)
        Determines random number generation for dataset creation. Pass an int
        for reproducible output across multiple function calls.
        See :term:`Glossary <random_state>`.

    Returns
    -------
    X : array of shape [n_samples, 10]
        The input samples.

    y : array of shape [n_samples]
        The output values.

    References
    ----------
    .. [1] T. Hastie, R. Tibshirani and J. Friedman, "Elements of Statistical
           Learning Ed. 2", Springer, 2009.

    See also
    --------
    make_gaussian_quantiles: a generalization of this dataset approach
    """
    rs = check_random_state(random_state)

    shape = (n_samples, 10)
    X = rs.normal(size=shape).reshape(shape)
    y = ((X ** 2.0).sum(axis=1) > 9.34).astype(np.float64, copy=False)
    y[y == 0.0] = -1.0

    return X, y
```