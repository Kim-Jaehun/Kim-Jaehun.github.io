---
layout: post
title:  "gunicorn gevent(Async)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---


### Gunicorn

Python WSGI HTTP Server이다.(Gunicorn은 Green Unicorn의 줄인말이라고 한다.)


### Gunicorn Async


Asynchronous workers consists of two types:
* Gevent
* Eventlet

both are python libraries based on the `Greenlet` library which implements the coroutines in python.

 gevent and eventlet both use `green threads`, in python green threads implements threading on the program level instead of implementing threads on the OS level. Threading on the program level will be considered a blocking program so it needs to use asynchronous I/O operations to overcome this problem.

<br>
##### What is coroutine(코루틴)
코루틴은 우리가 잘 알고 있는 서브루틴(Subroutine)과 달리 진입점(Entry Point)이 여러 개일 수 있습니다. 쉽게 이야기하면 실행을 멈췄다가(Suspend) 재개(Resume)할 수 있다는 점인데요. 이 특성을 살리면 우리가 익히 아는 스레드(Thread)처럼 쓸 수 있게 됩니다. 다만 스레드와 달리 코루틴은 비선점적(Non-Preemptive)이기때문에 코드의 흐름을 전적으로 사용자가 제어할 수 있습니다.

![aa](https://drive.google.com/uc?id=1LQjHTygXuEi641KefY7pe2ZrRQBVzXmY)
<br>








<br><br>
#### 참고문헌
* http://paphopu.tistory.com/entry/WSGI에-대한-설명-WSGI란-무엇인가
* https://spoqa.github.io/2012/02/13/concurrency-and-eventlet.html
