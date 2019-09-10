---
layout: post
title:  "[Machine_learning] Precision(정밀도), Recall(재현율), Accuracy(정확도) F1 score"
date:   2019-09-09 08:43:59
author: kim-jaehun
categories: algorithm
tags: machine_learning
sitemap :
  changefreq : daily
  priority : 1.0
comments: true
---

- Precision(정밀도), Recall(재현율), Accuracy(정확도) F1 score를 알아본다.

---


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


<br>
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





<br>
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

<br>
### F score

정밀도(Precision)과 재현율(Recall)의 가중 조화 평균

$$F_\beta = (1 + \beta^2) \, ({\text{precision} \times \text{recall}}) \, / \, ({\beta^2 \, \text{precision} + \text{recall}})$$

<br>
### F1 score

베타 $$\beta = 1$$

$$F_1 = 2 \cdot \text{precision} \cdot \text{recall} \, / \, (\text{precision} + \text{recall})$$




<br>
### 다중 클래스 Precision, Recall 계산하기 (Computing Precision and Recall for Multi-Class Classification)

* 실제 Precision, Recall 계산 방법

![aaaa](https://drive.google.com/uc?id=1ctDePY6sE79m5hT7w4E8crvV_7u6e9Sc)

* Precision
= TP_A/(Total predicted as A)
= TP_A/TotalPredicted_A
= 0.5
<br>
* Recall
= TP_A/(Total Gold for A)
= TP_A/TotalGoldLabel_A
= 0.3



<br><br>
#### 참고문헌
* http://text-analytics101.rxnlp.com/2014/10/computing-precision-and-recall-for.html
