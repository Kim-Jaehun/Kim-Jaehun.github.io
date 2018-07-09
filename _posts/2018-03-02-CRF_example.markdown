---
layout: post
title:  "CRF_example"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: machine learning
tags: CRF
---

##CRF example


- Conditional Random Fields is a discriminative undirected probabilistic graphical model, a sort of Markov random field.
- The most often used for NLP version of CRF is linear chain CRF
- CRF is a supervised learning method



####pycrfsuit Feature함수

``` python
features = [
    'bias',
    'word.lower=' + word.lower(),
    'word[-3:]=' + word[-3:],
    'word[-2:]=' + word[-2:],
    'word.isupper=%s' % word.isupper(),
    'word.istitle=%s' % word.istitle(),
    'word.isdigit=%s' % word.isdigit(),
    'postag=' + postag
]
```

####Tensorflow Feature함수
``` python
num_features = 100
x = np.random.rand(num_examples, num_words, num_features).astype(np.float32)
```


<br><br>
### Tensorflow에서의 loss 구현
```python
tf.contrib.crf.crf_log_likelihood(
    inputs,
    tag_indices,
    sequence_lengths,
    transition_params=None
)
```

#####Args:
* inputs: [batch_size, max_seq_len, num_tags]
* tag_indices: [batch_size, max_seq_len]
* sequence_lengths: [batch_size]

#####Returns:


* log_likelihood:[batch_size]
* transition_params: [num_tags, num_tags]

<br>
<br>
``` python
viterbi_sequence, viterbi_score = tf.contrib.crf.crf_decode( unary_scores, transition_params, sequence_lengths_t)
```


#####Args:
score:[seq_len, num_tags]
transition_params: [num_tags, num_tags]

#####Returns:
viterbi:[seq_len]
viterbi_score:  float




### 결과
#### 실제 Label
```python
[[2 1 1 3 1 0 1 4 0 1 4 3 2 4 1 2 3 4 0 3]
 [1 1 1 1 0 4 2 0 2 4 0 4 4 0 3 4 2 3 3 2]
 [2 4 2 1 1 4 2 1 3 2 3 2 3 3 2 1 4 0 3 2]
 [1 4 2 2 2 1 1 0 2 0 3 2 0 3 3 2 1 0 2 4]
 [2 1 2 2 4 0 0 2 1 3 0 2 0 2 0 0 1 0 0 3]
 [1 4 1 2 0 1 0 0 0 2 2 1 1 3 0 2 0 2 4 1]
 [4 0 2 0 2 4 4 1 0 2 2 2 3 4 2 4 0 3 4 1]
 [4 4 2 4 3 3 1 2 2 2 2 3 0 0 0 2 2 2 4 1]
 [1 2 4 4 1 2 3 4 2 1 0 1 4 4 1 2 1 1 1 3]
 [0 0 4 3 4 1 3 3 4 4 4 4 4 2 4 1 3 2 3 0]]
```

#### 학습 후 Label
```python
[[4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 2 3 0]
 [4 4 4 0 2 4 4 4 4 4 4 2 4 4 4 4 2 4 4 0]
 [4 4 4 4 0 2 3 2 4 0 2 4 4 4 2 4 4 4 4 0]
 [4 4 4 4 2 3 2 3 2 4 2 4 2 4 2 4 4 4 4 0]
 [4 4 4 4 4 4 4 2 4 4 4 4 4 4 4 2 4 2 4 0]
 [4 4 2 4 4 4 4 4 4 4 2 4 4 2 4 4 4 2 4 0]
 [4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 0]
 [4 4 4 4 4 4 4 4 4 4 4 2 4 2 4 4 4 4 4 0]
 [4 4 4 4 4 4 4 4 4 4 4 4 0 3 2 4 4 2 4 0]
 [3 0 2 4 4 4 4 4 2 4 4 2 4 4 4 4 4 4 4 0]]
 ...
 ...

 [[2 1 1 3 1 0 2 4 0 1 4 3 2 4 1 2 3 4 0 0]
 [1 1 1 1 0 3 2 0 2 4 4 4 4 0 3 4 2 3 3 0]
 [2 4 2 1 1 3 2 1 3 2 3 2 3 3 2 1 4 0 3 0]
 [1 4 2 4 2 1 1 0 2 0 3 2 0 2 3 2 1 0 2 0]
 [2 1 2 2 4 4 0 2 1 1 0 2 0 2 0 0 1 0 0 0]
 [1 4 4 2 0 1 0 0 0 2 2 1 1 3 0 2 0 2 4 0]
 [4 0 2 0 2 4 4 1 0 2 2 2 3 4 2 4 0 3 4 0]
 [4 4 2 4 3 3 3 2 2 2 2 3 0 0 0 2 2 2 1 0]
 [1 4 4 4 1 1 3 4 2 1 1 1 3 4 1 2 1 2 1 0]
 [0 0 4 3 2 2 3 3 4 4 4 4 4 2 4 1 3 2 3 0]]
```

초기에는 위와 같이 예측하다가 loss가 낮아질수록 비슷하게 예측하는 것을 확인
