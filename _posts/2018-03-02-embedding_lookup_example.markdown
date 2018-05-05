---
layout: post
title:  "embedding_lookup_example"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Deep Learning]
tags: tensorflow
---

# TextCNN

```python
import numpy as np
import tensorflow as tf

matrix = np.random.random([1024, 5])  # 5-dimensional embeddings
ids = np.array([0, 5, 17, 33])
print (matrix[ids])


W = tf.Variable(tf.random_uniform([1024, 5], -1.0, 1.0))
a = tf.nn.embedding_lookup(W, [0, 5, 17, 33])
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    print(sess.run(a))
```


```python

[[0.84965233 0.38010627 0.7802087  0.04073734 0.50221357]
 [0.55038719 0.03107117 0.1585652  0.75408777 0.48864533]
 [0.85355485 0.58459576 0.4010224  0.40606138 0.0696808 ]
 [0.92847228 0.30353077 0.42440735 0.1154842  0.30182662]]

[[ 0.02549362 -0.705992   -0.7031796  -0.11028695  0.00957942]
 [ 0.5189886  -0.039505   -0.45297503 -0.5531485   0.8404591 ]
 [-0.89544654  0.10968471 -0.16236877  0.21120214  0.9161763 ]
 [-0.9957292  -0.7955282   0.5413501   0.7636821   0.00398588]]


```
