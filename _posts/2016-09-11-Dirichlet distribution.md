---
layout: post
title: "Dirichlet distribution: Generating Random numbers that Sum to 1"
categories:
  - guide
tags:
  - content
---

## Dirichlet distribution: Generating Random numbers that Sum to 1
While working on automatic configuration generator, I needed to generate random number for parallelism level of different components of a system while keeping the total number of threads equal to the number of cores available. I found that a really good generic solution is to use [Dirichlet distribution](https://en.wikipedia.org/wiki/Dirichlet_distribution). There is a package available in python library [*numpy*](http://docs.scipy.org/doc/numpy/reference/generated/numpy.random.dirichlet.html). The function takes two parameters: a k-dimensional (k dimension for sample of dimension k) alpha parameter that sets the "randomness" of the samples and size parameter that determines the number of samples generated.

    >>> import numpy as np, numpy.random
    >>> k = 10
    >>> print np.random.dirichlet(np.ones(k),size=1)
    [[ 0.01779975  0.14165316  0.01029262  0.168136    0.03061161  0.09046587 0.19987289  0.13398581  0.03119906  0.17598322]]

In this case a k-dimensional alpha parameter that consists of all ones that passed to generate the samples. For alpha >> 1 the values get close to 1/k. While for alpha << 1 there is a bigger parity between the values for each of the k dimensions. For a visual reference of the affect of the value of alpha on the sample check this [link](http://stats.stackexchange.com/questions/95648/understanding-of-effect-of-alpha-in-dirichlet-distribution?newreg=123fd3d611b641338a2b0d68efb4b7ed)


**References**
- http://stackoverflow.com/questions/18659858/generating-a-list-of-random-numbers-summing-to-1/18662466#18662466
