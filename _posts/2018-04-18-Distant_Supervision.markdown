---
layout: post
title:  "Distant Supervision(원거리 감독법)"
date:   2018-06-1 08:43:59
author: Kim Jaehun
categories: [knowledge base population]
tags: KB
---


#Distant Supervision란?

책과 인터넷 등 자료를 스스로 읽어, 중요한 관계를 자동으로 추출 할 수 있는 기술은 자동으로 지식 베이스를 확장(knowledge base population)하는 기술이다.




### Distant Supervision(원거리 감독법)

```
문장 : {글로리아 스튜어트}는 폐암으로 투병하던 중 2010년 9월 16일 {캘리포니아 주}의 자택에서 사망했다.
```
```
트리플 : <글로리아 스튜어트 , deathPalce, 캘리포니아 주>
```

Distant Supervision(원거리 감독법)은 주어진 트리플에서, 해당 트리플이 포함하고 엔티티들은 함께 포함하고 있는 문장이 두 엔티티 사아의 관계를 표현하는 문장이라고 가정하는 휴리스틱 기법을 활용하여 모델 학습에 필요한 데이터를 스스로 생성.




<br>
### Distant Supervision(원거리 감독법)의 한계

* 트리플의 엔티티를 포함하고 잇는 문장이 트리플의 관계를 내포하고 있을 것이라는 가정 자체가 항상 옳은 것는 아니다.
<br>
* 동일한 관계(릴레이션) 가진 인스턴스들은 통합하여 하나의 일반화된 모델을 만드는 대신 복수의 템플릿화된 패턴의 형태의 모델을 사용한다.
<br>
* 이 패턴들의 신뢰를 측정하고 낮은 신되로를 가지는 패턴을 배제함으로써 잘못된 학습데이터로부터 기인하는 오류를 줄이고자 하였다.






<br><br>
#### 참고문헌


* http://pail.unist.ac.kr/papers/Slot_Filling_KISSE.pdf(지식베이스 확장을 위한 자동 관계 추출)

* (지식베이스 확장을 위한 트리플 추출)

* http://kiise.or.kr/e_journal/2016/6/JOK/pdf/06.pdf(의미 유사도를 활용한 Distant Supervision 기반의 트리플 생성 성능 향상)
