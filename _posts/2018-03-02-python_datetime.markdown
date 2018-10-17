---
layout: post
title:  "python datetime(날짜 순서대로 출력)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

시간 순으로 출력하기

```python
from datetime import timedelta, datetime

start_date = datetime(2018, 1, 2, 0, 0, 0)
for td in (start_date + timedelta(minutes=60*it) for it in range(25)):
    curtime = td.strftime("%Y-%m-%d-%H")
    print(curtime)
```

```
2018-01-02-00
2018-01-02-01
2018-01-02-02
2018-01-02-03
2018-01-02-04
2018-01-02-05

...
2018-01-02-22
2018-01-02-23
2018-01-03-00
```




<br><br>
#### 참고문헌
