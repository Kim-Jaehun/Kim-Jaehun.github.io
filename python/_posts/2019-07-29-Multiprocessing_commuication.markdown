---
layout: post
title:  "Multiprocessing communication"
date:   2019-07-30 08:43:59
author: kim-jaehun
categories: [python]
tags: [python]
sitemap :
  changefreq : daily
  priority : 1.0
---

##Multiprocessing communication

프로세스 간의 값의 communication이 필요할 떄 `Queue`와 `PIPE`를 제공하고 있습니다.

`Pipe()` can only have two endpoints.
`Queue()` can have multiple producers and consumers.

Pipe가 Queue보다 빠르다.


 프로세스가 종료되면 그것의 자식 데몬 프로세스들을 강제종료 시킬 것이다. 데몬 프로세스는 자식 프로세스를 생성 할 수 없으며, 유닉스 데몬이나 서비스가 아니다. 디폴트인 넌데몬프로세스 (daemon = False) 일 경우에는 해당 프로세스가 종료 될 때까지 메인프로세스는 종료되지 않는다. 암시적으로 내부에서 join() 하고 있다.  
참고로 메인프로세스가 갑자기 죽은 경우 데몬 자식 프로세스를 종료하지 못한다.

### Queue

```python
from multiprocessing import Process, Queue

def f(q, name):
	q.put(name)

if __name__ == '__main__':
	q = Queue()
	p1 = Process(target=f, args=(q, 'aaa'))
	p2 = Process(target=f, args=(q, 'bbb'))

	p1.start()
	p2.start()

	p1.join()
	p2.join()

	print(q.qsize())

```

### Pipe
파이프 오브젝트는 서로 연결된 한쌍의 오브젝트로 이루어지며, 한쪽에서 다른쪽으로 데이터를 보낼 수 있다.

```python
from multiprocessing import Process, Pipe


def f(conn):
	print(conn.recv())
	conn.send(['Chihaya', 72, 'keut'])
	conn.close()


if __name__ == '__main__':
	parent_conn, child_conn = Pipe()
	parent_conn.send(['amami', 'haruka', 'boss'])

	# print(child_conn.recv())
	p = Process(target=f, args=(child_conn,))
	p.start()
	print(parent_conn.recv())
	p.join()
  ```




<br><br>
#### 참고문헌
* https://stackoverflow.com/questions/8463008/multiprocessing-pipe-vs-queue
* https://hamait.tistory.com/755
