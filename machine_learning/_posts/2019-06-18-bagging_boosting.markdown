---
layout: post
title:  "Bagging Boosting"
date:   2019-06-18 08:43:59
author: kim-jaehun
categories: [machine_learning]
tags: [machine_learning]
sitemap :
  changefreq : daily
  priority : 1.0
---

## Bagging과 Boosting

**앙상블 기법**

`앙상블`이란 프랑스어이며 뜻은 조화 또는 통일을 의미한다.

머신러닝에서 여러개의 모델을 학습시켜 그 모델들의 예측결과들을 이용해 하나의 모델보다 더 나은 값을 예측하는 방법을 말한다. 이러한 방법을 앙상블 학습(ensemble learning) 또는 앙상블 방법(ensemble method)이라고 한다.

앙상블 기법의 예로,
 - Bagging
 - Boosting
 - Random Forest
 - Stacking

이 중 Bagging과 Boosting 에 대해서 알아보자.

![single bagging boosting](https://drive.google.com/uc?id=1USs0-aBCbbeLqV7j0l5TZcyfLg9BG51I)

<br>
### Bagging

 - `Bagging`이란 `Bootstrap Aggregating`의 줄임말이다.
 - 데이터를 랜덤 `random samping` 통해 크기가 동일한 여러 개의 표본 자료를 생성해, 각각에 대한 분류기를 생성하고, 분류기의 결과를 종합하여 의사결정을 내린다.
(이 떄, 최종 결과값이 연속형일 경우 `average`을 사용하고, 범주형(카테고리형)일 경우 `majority vote`를 통해 결정)

<br>
### Boosting

- `boosting`은 성능이 약한 학습기(weak learner)를 여러 개 연결하여 강한 학습기(strong learner)를 만드는 앙상블 학습이다.
- 모델링을 통한 예측변수에 의해 오분류된 개체들에게 높은 가중치를 부여하고, 정분류된 객체들에는 낮은 가중치를 부여하는 방식


#### 참고문헌
* https://excelsior-cjh.tistory.com/166
* https://m.blog.naver.com/muzzincys/220201299384
