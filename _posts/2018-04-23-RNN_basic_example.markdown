---
layout: post
title:  "RNN basic example"
date:   2018-04-23 13:43:59
author: kim-jaehun
categories:
  - Deep Learning
tags:  'RNN'
use_math: true
---
## RNN basic

### Data

{% highlight python3 %}
x_data = [[
            [1, 0, 0, 0],
            [0, 1, 0, 0],
            [0, 0, 1, 0]
        ]]
{% endhighlight %}

데이터는 one-hot encoding으로 되어있다.

batch_size = 1

sequence_length = 3

embedding_size = 4

"Hello world !" -->  문장 1개

"Hello" , "world" , "!"  --> 단어 3개

각 단어의 백터의 사이즈 = 4


### RNN

{% highlight python3 %}
with tf.variable_scope('one_cell') as scope:

    X = tf.placeholder(tf.float32, [1, 3, 4])
    hidden_size = 2
    cell = tf.contrib.rnn.BasicRNNCell(num_units=hidden_size)
    outputs, states = tf.nn.dynamic_rnn(cell, X, dtype=tf.float32)

    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        outputs, states = sess.run([outputs, states], feed_dict={X: x_data})

        print(outputs)
        print(states)

{% endhighlight %}

hidden_size = 2 로 설정함으로써,  4 차원 --> 2차원으로 축소했다고 생각할 수 있다.

여기서 cell이 , 이전의 값들을 기억하고 있다.


{% highlight python3 %}
"""
[[[-0.5356819  -0.67356485]
  [ 0.4196428  -0.8449874 ]
  [-0.85603344 -0.6445809 ]]]

[[-0.85603344 -0.6445809 ]]
"""
{% endhighlight %}

결과를 보게 되면 time 마다 outputs 을 볼 수 있다.

states 는 마지막 cell 값(이전의 값들을 기억)을 가지고 있다.



### batch_size 2인 RNN



{% highlight python3 %}
x_data = [[
            [1, 0, 0, 0],
            [0, 1, 0, 0],
            [0, 0, 1, 0]
        ],[
            [0, 0, 0, 1],
            [0, 1, 0, 0],
            [0, 0, 1, 0]

        ]]
{% endhighlight %}


{% highlight python3 %}
with tf.variable_scope('2_batches_dynamic_length') as scope:

    hidden_size = 2
    X = tf.placeholder(tf.float32, [None, 3, 4])
    cell = rnn.BasicRNNCell(num_units=hidden_size)
    outputs, states = tf.nn.dynamic_rnn(cell, X,  dtype=tf.float32)

    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        outputs, states = sess.run([outputs, states], feed_dict={X:x_data})

"""
[[[ 0.0508305   0.6284012 ]
  [-0.5467192   0.5789988 ]
  [-0.84925133  0.34580162]]

 [[ 0.12604629 -0.43704653]
  [ 0.14376211  0.28574872]
  [-0.5611005   0.40640762]]]
**********
[[-0.84925133  0.34580162]
 [-0.5611005   0.40640762]]
"""
{% endhighlight %}


### initial_state가 있는 경우 RNN

initial_state란?

초기에 RNN에 $$h_{t-1}$$ 가 임의로 지정.

{% highlight python3 %}
with tf.variable_scope('initial_state') as scope:

    X = tf.placeholder(tf.float32, [2, 3, 4])

    hidden_size = 2
    cell = rnn.BasicRNNCell(num_units=hidden_size)
    initial_state = cell.zero_state(2, tf.float32)
    outputs, states = tf.nn.dynamic_rnn(cell, X, initial_state=initial_state, dtype=tf.float32)
    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        outputs, states = sess.run([outputs, states], feed_dict={X:x_data})

{% endhighlight %}


### MultiRNN

{% highlight python3 %}

with tf.variable_scope('MultiRNNCell') as scope:
    X = tf.placeholder(tf.float32, [1, 3, 4])
    cell = rnn.BasicRNNCell(num_units=3)
    cell2 = rnn.BasicRNNCell(num_units=5)
    cell3 = rnn.BasicRNNCell(num_units=6)
    cells = rnn.MultiRNNCell([cell, cell2, cell3])
    outputs, states = tf.nn.dynamic_rnn(cells, X, dtype=tf.float32)
    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        outputs, states = sess.run([outputs, states], feed_dict={X: x_data})

        print(outputs)
        print('**********')
        print(states)

{% endhighlight %}

3-layer

output의 출력은 마지막 hidden-siz3=6
마지막 states 3, 5, 6 각각의 cell의 값이 출력.

{% highlight python3 %}
"""
[[[ 0.10490837 -0.25118724  0.01415536 -0.26365766  0.29573244
    0.2170027 ]
  [ 0.61581135  0.14419231 -0.36419457  0.48796558 -0.09259523
   -0.3199411 ]
  [-0.34976253 -0.18271485 -0.5188264   0.13098443 -0.65751684
   -0.47144717]]]
**********
(array([[ 0.856855  , -0.5598132 ,  0.55492365]], dtype=float32),
array([[-0.46948567, -0.3382116 , -0.6107864 , -0.55326957,  0.6156729 ]],  dtype=float32),
array([[-0.34976253, -0.18271485, -0.5188264 ,  0.13098443, -0.65751684, -0.47144717]], dtype=float32))
"""
{% endhighlight %}



### bi-directional

![bi-directional](https://drive.google.com/uc?id=1GrwcGT8QaJdzBbsTggxnpjchu-UmN6gB)

{% highlight python3 %}
with tf.variable_scope('bi-directional') as scope:
    X = tf.placeholder(tf.float32, [1, 3, 4])
    cell_fw = rnn.BasicRNNCell(num_units=2)
    cell_bw = rnn.BasicRNNCell(num_units=2)
    outputs, states = tf.nn.bidirectional_dynamic_rnn(cell_fw, cell_bw, X, dtype=tf.float32)
    with tf.Session() as sess:
        sess.run(tf.global_variables_initializer())
        outputs, states = sess.run([outputs, states], feed_dict={X: x_data})

        print(outputs)
        print('**********')
        print(states)
{% endhighlight %}

* bi-directional

output의 출력은 마지막 hidden-siz3=6
마지막 states 3, 5, 6 각각의 cell의 값이 출력

{% highlight python3 %}
"""
(array([[[-0.11544231, -0.6719475 ], [ 0.64414954, -0.41345194], [ 0.7955334 ,  0.10082521]]], dtype=float32),
array([[[ 0.26266363, -0.7163632 ], [-0.02817043, -0.88044846], [ 0.31503847, -0.5990384 ]]], dtype=float32))
**********

(array([[0.7955334 , 0.10082521]], dtype=float32),
array([[ 0.26266363, -0.7163632 ]], dtype=float32))

"""
{% endhighlight %}
