---
layout: post
title:  "파이썬 딕셔너리(dictionary) 정렬하기"
date:   2019-09-09 08:43:59
author: kim-jaehun
categories: python
tags: python
sitemap :
  changefreq : daily
  priority : 1.0
comments: true
---

- 파이썬의 자료형 딕셔너리의 정렬에 대해서 알아본다.

---

## 파이썬 딕셔너리 정렬


#### 방법 1 operator 이용
```python

import operator

x = {'a':5, 'c':3, 'd':3, 'b':4, 'e':1}

sorted_by_key = sorted(x.items(), key=operator.itemgetter(0))
print(sorted_by_key)
sorted_by_value = sorted(x.items(), key=operator.itemgetter(1))
print(sorted_by_value)

#[('a', 5), ('b', 4), ('c', 3), ('d', 3), ('e', 1)]
#[('e', 1), ('c', 3), ('d', 3), ('b', 4), ('a', 5)]

```

#### 방법 2 lambda 이용
```python

x = {'a':5, 'c':3, 'd':3, 'b':4, 'e':1}

sorted_by_key = sorted(x.items(), key=lambda a:a[0])
print(sorted_by_key)
sorted_by_value = sorted(x.items(), key=lambda a:a[1])
print(sorted_by_value)

#내림차순으로 하고 싶다면
#sorted_by_value = sorted(x.items(), key=lambda a:a[1], reverse=True)

#[('a', 5), ('b', 4), ('c', 3), ('d', 3), ('e', 1)]
#[('e', 1), ('c', 3), ('d', 3), ('b', 4), ('a', 5)]
```


<br><br>
#### 참고문헌
* https://stackoverflow.com/questions/613183/how-do-i-sort-a-dictionary-by-value
