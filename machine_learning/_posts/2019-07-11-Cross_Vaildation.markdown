---
layout: post
title:  "Cross Vaildation"
date:   2019-07-11 08:43:59
author: kim-jaehun
categories: [machine_learning]
tags: [machine_learning]
sitemap :
  changefreq : daily
  priority : 1.0
---

## 교차 검증(Cross Vaildation)

#### K-fold Cross Vaildation

```python
import numpy as np
from sklearn.model_selection import KFold
X = np.array([[1, 2], [3, 4], [1, 2], [3, 4], [4,5], [5,6]])
y = np.array([1, 2, 3, 4, 5, 6])
#kf = KFold(n_splits=6, random_state=None, shuffle=False)
kf = KFold(n_splits=3, random_state=None, shuffle=False)

for train_index, test_index in kf.split(X):
    print("TRAIN:", train_index, "TEST:", test_index)

#TRAIN: [2 3 4 5] TEST: [0 1]
#TRAIN: [0 1 4 5] TEST: [2 3]
#TRAIN: [0 1 2 3] TEST: [4 5]
```
![KFold](https://drive.google.com/uc?id=1wZxAmIlgkGGl2ln9YeSH1ktLx844G8_K)

총 K개의 모형과 K개의 교차검증 성능이 나온다. 이 K개의 교차검증 성능을 평균하여 최종 교차검증 성능을 계산한다.


#### StratifiedKFold

```python
import numpy as np
from sklearn.model_selection import StratifiedKFold
X = np.array([[1, 2], [3, 4], [1, 2], [3, 4], [4,5], [5,6],[6,7],[7,8],[8,9],[9,10],[11,12],[12,13]])
y = np.array([1,1,1,1,1,1,1,1,2,2,2,2])
skf = StratifiedKFold(n_splits=2, random_state=None, shuffle=False)

for train_index, test_index in skf.split(X, y):
    print("TRAIN:", train_index, "TEST:", test_index)

#TRAIN: [ 4  5  6  7 10 11] TEST: [0 1 2 3 8 9]
#TRAIN: [0 1 2 3 8 9] TEST: [ 4  5  6  7 10 11]
```

![StratifiedKFold](https://drive.google.com/uc?id=1jDaxUTSBkR86n1MyMVHJTPobdfDsHB6z)

총 K개의 모형과 K개의 교차검증 성능이 나온다. 각 클래스 존재하는 비율에 맞게 검증 셋을 만든다. 이 K개의 교차검증 성능을 평균하여 최종 교차검증 성능을 계산한다.


#### StratifiedShuffleSplit

```python
import numpy as np
from sklearn.model_selection import StratifiedShuffleSplit
X = np.array([[1, 2], [3, 4], [1, 2], [3, 4], [4,5], [5,6],[6,7],[7,8],[8,9],[9,10],[11,12],[12,13]])
y = np.array([1,1,1,1,1,1,1,1,2,2,2,2])
skf = StratifiedShuffleSplit(n_splits=2, random_state=None, test_size=0.3)

for train_index, test_index in skf.split(X, y):
    print("TRAIN:", train_index, "TEST:", test_index)

#TRAIN: [11  7  5 10  1  4  9  0] TEST: [6 3 2 8]
#TRAIN: [11  7 10  6  2  8  5  0] TEST: [1 3 9 4]
```
![StratifiedShuffleSplit](https://drive.google.com/uc?id=1HghH4tqjD2ZATrGMjWlkcCK4644Vt2nw)

StratifiedKFold과의 차이점은 각 폴트 마다 shuffle이 된다. 각 폴트의 테스트 셋이 겹칠 수 있습니다.

<br><br>
#### 참고문헌
*
