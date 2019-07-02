---
layout: post
title:  "Decision Tree"
date:   2019-05-20 08:43:59
author: kim-jaehun
categories: [machine_learning]
tags: [machine_learning]
sitemap :
  changefreq : daily
  priority : 1.0
---

## 의사결정나무(Decision Tree)

- 어떤 항목에 대한 관측값과 목표값을 연결시켜주는 예측 모델로써 결정 트리를 사용한다.
- 분류(`classification`) 와 회귀(`Regression`)에 사용되는 비모수적(`non-parametric`) 감독 학습이다.
- CART(Classification And Regression Tree)라고도 한다.

<br>
### Decision Tree Classification

``` python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
import numpy as np

iris = load_iris()
X = iris.data[:, 2:4]
y = iris.target

# X = [[1.4 0.2] [1.4 0.2] [1.3 0.2] [1.5 0.2] ... ]
# y = [0 0 0 0 ...] 0 or 1 or 2

classification = DecisionTreeClassifier(random_state=0,
max_leaf_nodes=2).fit(X, y)

res = []
for i in np.arange(0, 6, 0.1):
    res.append(classification.predict([[i, i]]))
print(res)

# [array([0]), array([0]), ...., array([1]), array([1]), ...array([1])]
```

`max_leaf_nodes=2`로 함으로써  실제 데이터에서는 `target = 0 or 1 or 2`를 가지지만 예측결과는 `target = 0 or 1`로 나누어 졌다.
분류(Classification)의 경우는 새로운 데이터가 특정 terminal node에 속한다는 정보를 확인한 뒤, terminal node에서 가장 빈도가 높은 범주에 새로운 데이터를 분류하게 된다.

<br>
### Decision Tree Regression

```python
from sklearn.tree import DecisionTreeRegressor  

import numpy as np

dataset = np.array(
[['Asset Flip', 100, 1000],
['Text Based', 500, 3000],
['Visual Novel', 1500, 5000],
['2D Pixel Art', 3500, 8000],
['2D Vector Art', 5000, 6500],
['Strategy', 6000, 7000],
['First Person Shooter', 8000, 15000],
['Simulator', 9500, 20000],
['Racing', 12000, 21000],
['RPG', 14000, 25000],
['Sandbox', 15500, 27000],
['Open-World', 16500, 30000],
['MMOFPS', 25000, 52000],
['MMORPG', 30000, 80000]
])
X = dataset[:, 1:2].astype(int)  
y = dataset[:, 2].astype(int)  
regressor = DecisionTreeRegressor(random_state = 0, max_leaf_nodes=10).fit(X, y)

print(regressor.predict(3750))
# 7166

```

`max_leaf_nodes=10`으로 함으로써, target 값에 존재하지 않은 평균 예측값이다.
회귀(Regression)의 해당 terminal node의 종속변수(y)의 평균을 예측값으로 반환한다.



### Decision Tree

분류 나무는 구분 뒤 각 영역의 순도(homogeneity)가 증가, 불순도(impurity) 혹은 불확실성(uncertainty)이 최대한 감소하도록 하는 방향으로 학습을 진행합니다.




<br><br>
#### 참고문헌
* https://ko.wikipedia.org/wiki/%EA%B2%B0%EC%A0%95_%ED%8A%B8%EB%A6%AC_%ED%95%99%EC%8A%B5%EB%B2%95
* https://scikit-learn.org/stable/modules/tree.html
* https://ratsgo.github.io/machine%20learning/2017/03/26/tree/
