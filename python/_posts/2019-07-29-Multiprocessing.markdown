---
layout: post
title:  "Multiprocessing"
date:   2019-07-29 08:43:59
author: kim-jaehun
categories: [python]
tags: [python]
sitemap :
  changefreq : daily
  priority : 1.0
---

##Mutiprocessing

mutiprocessing 에서는 `Pool` 과 `Process` 를 이용하여 하나 이상의 자식 process를 생성하여, 병렬구조로 처리합니다.

##Pool

입력값을 process들을 건너건너 분배하여 함수실행의 병렬화 하는 편리한 수단을 제공한다.


```python
from multiprocessing import Pool
import os

def doubler(number):
    result = number * 2
    proc = os.getpid()
    print('{0} doubled to {1} by process id: {2}'.format(number, result, proc))

if __name__ == '__main__':
    p = Pool(3)
    p.map(doubler, range(0,10))


#0 doubled to 0 by process id: 19162
#1 doubled to 2 by process id: 19163
#2 doubled to 4 by process id: 19164
#3 doubled to 6 by process id: 19162
#4 doubled to 8 by process id: 19164
#5 doubled to 10 by process id: 19163
#6 doubled to 12 by process id: 19164
#7 doubled to 14 by process id: 19162
#8 doubled to 16 by process id: 19163
#9 doubled to 18 by process id: 19162
```

PID `19162`, `19163` ,`19164` 3개의 프로세스들이 처리하는 하고 있다.

##Process

Process 는 하나의 프로세스를 하나의 함수에 적당한 인자값을 할당 또는 할당하지 않고 실행합니다.

```python
import os
from multiprocessing import Process

def doubler(number):
    result = number * 2
    proc = os.getpid()
    print('{0} doubled to {1} by process id: {2}'.format(number, result, proc))

if __name__ == '__main__':
	numbers = [0,1,2]
	procs = []

	for index, number in enumerate(numbers):
		proc = Process(target=doubler, args=(number,))
		procs.append(proc)
		proc.start()

	for proc in procs:
		proc.join()

#0 doubled to 0 by process id: 19439
#1 doubled to 2 by process id: 19440
#2 doubled to 4 by process id: 19441

```
PID `19439`, `19440` ,`19441` 3개의 프로세스들이 처리하는 하고 있다.

`pool.close` tells the pool not to accept any new job.
`pool.join` tells the pool to wait until all jobs finished then exit, effectively cleaning up the pool.
`pool.terminate()` Stops the worker processes immediately without completing outstanding work.


<br><br>
#### 참고문헌
* https://m.blog.naver.com/townpharm/220951524843
