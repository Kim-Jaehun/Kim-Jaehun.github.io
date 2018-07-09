---
layout: post
title:  "Relation Extraction"
date:   2018-06-1 08:43:59
author: Kim Jaehun
categories: [Relation_Extraction]
tags: Relation_Extraction
---


##Relation Extraction(관계 추출)?

* 비구조적인 자연어 문자응로부터 구조적인 트리플(triple)을 추출하는 작업

* 두 개체 간의 관계(relation) 를 <주어, 관계, 목적어>와 같이 세 개의 항으로 표현





## Neural Relation Extraction(NRE)

 - word-level + sentence-level

 - Bidirectional GRU network (BGRU+2ATT).

[thunlp.github](https://github.com/thunlp/TensorFlow-NRE)을 상세하게 소스코드를 분석


### Data

NYT10이라는 DataSet 사용

데이터 포맷 fb_mid_e1 \t fb_mid_e2 \t e1_name \t e2_name \t relation \t sentence

- e1_name = 엔티티1 = queens
- e2_name = 엔티티2 = belle_harbor
- relation = 관계
- sentence = 문장

```
m.0ccvx	m.05gf08	queens	belle_harbor	/location/location/contains	sen. charles e. schumer called on federal safety officials yesterday to reopen their investigation into the fatal crash of a passenger jet in belle_harbor , queens , because equipment failure , not pilot error , might have been the cause . ###END###
```

belle_harbor(벨 하버)는  뉴욕시 queens(퀸즈) 자치구의 동네입니다.(belle_harbor < queens)


### initial_data(data_heleper)

initial.py을 먼저 실행해줘야한다.

[[[[3, 53, 39], [517, 54, 40], [14, 55, 41], [2123, 56, 42], [15, 57, 43],

('new_jersey', 'wanaque_river')






<br><br>
#### 참고문헌


* http://semanticweb.kaist.ac.kr/home/images/7/74/%EA%B4%80%EA%B3%84%EC%B6%94%EC%B6%9C_%EB%AA%A8%EB%8D%B8_%ED%95%99%EC%8A%B5%EC%9D%84_%EC%9C%84%ED%95%9C_%EB%B0%98%EC%9E%90%EB%8F%99_%ED%8C%A8%ED%84%B4_%EB%A7%88%EC%9D%B4%EB%8B%9D.pdf

* https://github.com/thunlp/TensorFlow-NRE
