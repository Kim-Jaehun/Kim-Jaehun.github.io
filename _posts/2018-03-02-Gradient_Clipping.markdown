---
layout: post
title:  "Gradient Clipping(그래디언트 클리핑)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: Dummy
tags: lorem ipsum
---

## Gradient Clipping(그래디언트 클리핑)

* 그래디언트 폭주 문제를 줄이는 쉬운 방법은 역전파될때 일정 임계값을 넘어서지 못하게 그래디언트를 단순하네 잘라내는 것입니다.
<br>
* RNN 등 매우 깊은 뉴럴넷에서 그래디언트가 폭발하는 걸 방지하기 위한 기법입니다. 다양한 방법이 존재하지만 흔히 쓰이는 방법은 그래디언트의 L2 norm이 기준값을 초과할 때 (threshold / L2 norm)을 곱해주는 것입니다.


``` python
tf.clip_by_value(num, min, max)
```
<br>
``` python
optim = tf.train.AdamOptimizer(learning_rate=self.lr_pl)
grads_and_vars = optim.compute_gradients(self.loss)
grads_and_vars_clip = [[tf.clip_by_value(g, -self.clip_grad, self.clip_grad), v] for g, v in grads_and_vars]
self.train_op = optim.apply_gradients(grads_and_vars_clip, global_step=self.global_step)
```

<br><br>
#### 참고문헌


* http://wizardsnote.tumblr.com/post/138951940375/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EB%8B%A8%EC%96%B4%EC%82%AC%EC%A0%84-%ED%8A%B8%EB%A6%AD-%EB%AA%A8%EC%9D%8C-%EB%B0%8F-%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4
