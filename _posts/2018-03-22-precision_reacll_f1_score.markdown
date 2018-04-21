---
layout: post
title:  "Precision, Recall, F1 Score"
date:   2018-03-22 13:43:59
author: kim-jaehun
categories: Deep_Learning
tags: Deep_Learning word2vec
use_math: true
---
## 평가


### Precision (정밀도)

정밀도는 검색된 결과들 중 관련 있는 것으로 분류된 결과물의 비율

$${TP \over TP + FP}$$

<table>
  <thead>
    <tr>
      <th></th>
      <th>predict_POSTIVE</th>
      <th>predict_NEGATIVE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>label_POSTIVE</td>
      <td bgcolor="#00FF00" >true-postive</td>
      <td bgcolor="#00FF00" >false-negative</td>
    </tr>
    <tr>
      <td>label_NEGATIVE</td>
      <td>false-postvie</td>
      <td>true-negative</td>
    </tr>
  </tbody>
</table>





### Recall (재현율)

재현율은 관련 있는 것으로 분류된 항목들 중 실제 검색된 항목들의 비율

$${TP \over TP + FN}$$

<table>
  <thead>
    <tr>
      <th></th>
      <th>predict_POSTIVE</th>
      <th>predict_NEGATIVE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>label_POSTIVE</td>
      <td bgcolor="#00FF00">true-postive</td>
      <td >false-negative</td>
    </tr>
    <tr>
      <td>label_NEGATIVE</td>
      <td bgcolor="#00FF00" >false-postvie</td>
      <td>true-negative</td>
    </tr>
  </tbody>
</table>





### Accuracy (정확도)

$${TP \over TP + TN + FP + FN}$$

<table>
  <thead>
    <tr>
      <th></th>
      <th>predict_POSTIVE</th>
      <th>predict_NEGATIVE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>label_POSTIVE</td>
      <td bgcolor="#00FF00" >true-postive</td>
      <td bgcolor="#00FF00">false-negative</td>
    </tr>
    <tr>
      <td>label_NEGATIVE</td>
      <td bgcolor="#00FF00" >false-postvie</td>
      <td bgcolor="#00FF00" >true-negative</td>
    </tr>
  </tbody>
</table>

### F score

정밀도(Precision)과 재현율(Recall)의 가중 조화 평균

$$F_\beta = (1 + \beta^2) \, ({\text{precision} \times \text{recall}}) \, / \, ({\beta^2 \, \text{precision} + \text{recall}})$$


### F1 score

베타 $$\beta$$  = 1

$$F_1 = 2 \cdot \text{precision} \cdot \text{recall} \, / \, (\text{precision} + \text{recall})$$


{% highlight python3 %}

from sklearn.metrics import *

y_true = [0, 1, 2, 2, 2]
y_pred = [0, 0, 2, 2, 1]
target_names = ['class 0', 'class 1', 'class 2']
print(classification_report(y_true, y_pred, target_names=target_names))

"""
precision    recall  f1-score   support

    class 0       0.50      1.00      0.67         1
    class 1       0.00      0.00      0.00         1
    class 2       1.00      0.67      0.80         3

avg / total       0.70      0.60      0.61         5
"""

{% endhighlight %}

### (precision, recall, f1 score) Average

각 클래스별 (precision, recall , f1 score ), count를 출력.

{% highlight python3 %}

from sklearn.metrics import precision_recall_fscore_support

y_true = np.array(['cat', 'dog', 'pig', 'cat', 'dog', 'pig', 'aaa'])
y_pred = np.array(['cat', 'pig', 'dog', 'cat', 'cat', 'dog', 'dog'])

print(precision_recall_fscore_support(y_true, y_pred))

"""
(array([1.  , 0.25, 0.  ]), array([1. , 0.5, 0. ]), array([1.        , 0.33333333, 0.        ]), array([2, 2, 3]))
"""
{% endhighlight %}
