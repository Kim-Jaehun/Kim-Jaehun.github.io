---
layout: post
title:  "Bias Variance Trade-off"
date:   2019-06-17 08:43:59
author: kim-jaehun
categories: algorithm
tags: machine_learning
sitemap :
  changefreq : daily
  priority : 1.0
---

## 편향-분산 트레이드 오프(Bias Variance Trade-off)

통계학과 기계 학습 분야에서 말하는 편향-분산 트레이드오프(Bias-variance tradeoff) (또는 딜레마(dilemma))는 지도 학습 알고리즘이 트레이닝 셋의 범위를 넘어 지나치게 일반화 하는 것을 예방하기 위해 두 종류의 오차(편향, 분산)를 최소화 할 때 겪는 문제이다.

<br>
### 편향(Bias)이란?
편향은 학습 알고리즘에서 잘못된 가정을 했을 때 발생하는 오차이다. 높은 편향값은 알고리즘이 데이터의 특징과 결과물과의 적절한 관계를 놓치게 만드는 과소적합(underfitting) 문제를 발생 시킨다.

<br>
### 분산(Variance)이란?
분산은 트레이닝 셋에 내재된 작은 변동(fluctuation) 때문에 발생하는 오차이다. 높은 분산값은 큰 노이즈까지 모델링에 포함시키는 과적합(overfitting) 문제를 발생 시킨다.

<br>
### Bias and variance using bulls-eye diagram
![Bias and variance using bulls-eye diagram](https://drive.google.com/uc?id=1g7xKn3y2-0cZKuvgrAjY2nrt0Q9156A5)

위의 그림은 bulls-eye diagram을 통해서 쉽게 Bias와 Variance를 표현하고 있니다.

* Underfitting은 모델이 데이터의 기본 패턴을 포착 할 수 없을 때 발생합니다. 이 모델은 일반적으로 편향이 크고(`high bias`) 분산가 적습니다(`low variance`).

* overfitting은 모델이 데이터의 기본 패턴과 함께 노이즈를 포착할 때 발생합니다. 노이즈가 많은 모델을 학습할 떄 발생합니다. 이 모델은 편향이 작고(`low bias`) 분산이 큽니다(`high variance`).

![Bias and variance example](https://drive.google.com/uc?id=1t8cBVneRauDc0z6iY-G4Slj2cd470-EC)

<br>
### 결론

우리 모델이 너무 단순하고 파라미터가 거의 없다면 편향이 크고 분산이 낮을 수 있습니다.
반면에 우리 모델에 많은 수의 매개 변수가 있다면 높은 분산과 낮은 편차를 가질 것입니다.
따라서 우리는 데이터를 오버피팅 시키지 않고 올바른 / 좋은 균형을 찾아야합니다.

**Total Error = Bias^2 + Variance + irreducible Error**
 위의 식은 [제곱 오류의 편향-분산 분해](https://ko.wikipedia.org/wiki/%ED%8E%B8%ED%96%A5-%EB%B6%84%EC%82%B0_%ED%8A%B8%EB%A0%88%EC%9D%B4%EB%93%9C%EC%98%A4%ED%94%84)와 [Understanding the Bias-Variance Tradeoff](https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229)을 참고해서 보면 유도할 수 있다.

![Total Error = Bias^2 + Variance + irreducible Error Graph](https://drive.google.com/uc?id=1fkUZqaqV6HDF04lpTwP9tPft6Vaxv4AX)







 <br><br>
 #### 참고문헌
 * https://ko.wikipedia.org/wiki/%ED%8E%B8%ED%96%A5-%EB%B6%84%EC%82%B0_%ED%8A%B8%EB%A0%88%EC%9D%B4%EB%93%9C%EC%98%A4%ED%94%84
* https://towardsdatascience.com/understanding-the-bias-variance-tradeoff-165e6942b229
