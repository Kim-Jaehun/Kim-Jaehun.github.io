---
layout: post
title:  "Flask Gunicorn Performance Testing"
date:   2019-07-01 08:43:59
author: kim-jaehun
categories: [python]
tags: [python]
sitemap :
  changefreq : daily
  priority : 1.0
---


## Flask Performance Testing

`Flask` 앱을 만든다.

```python
from flask import Flask
import time
app = Flask(__name__)

@app.route('/')
def hello_world1():
	time.sleep(1)
	return 'hello world'

if __name__ == "__main__":
    app.run(debug=True)
```

그 다음 `ab`을 이용해 성능을 체크해보자. request를 1개씩(-c 1) 100개(-n 100)를 보낸다.
```
# ab -n 100 -c 1 127.0.0.1:5000/


Benchmarking 127.0.0.1 (be patient).....done


Server Software:        Werkzeug/0.14.1
Server Hostname:        127.0.0.1
Server Port:            5000

Document Path:          /test
Document Length:        11 bytes

Concurrency Level:      1
Time taken for tests:   100.273 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      16500 bytes
HTML transferred:       1100 bytes
Requests per second:    1.00 [#/sec] (mean)
Time per request:       1002.733 [ms] (mean)
Time per request:       1002.733 [ms] (mean, across all concurrent requests)
Transfer rate:          0.16 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.0      0       0
Processing:  1001 1003   0.3   1003    1003
Waiting:     1001 1002   0.3   1002    1003
Total:       1001 1003   0.4   1003    1003

Percentage of the requests served within a certain time (ms)
  50%   1003
  66%   1003
  75%   1003
  80%   1003
  90%   1003
  95%   1003
  98%   1003
  99%   1003
 100%   1003 (longest request)

```

기본적으로 Flask앱은 `single thread`로 동작하기 때문에 한번에 하나의 request만을 처리한다.
이번엔 request를 10개씩(-c 10) 100개(-n 100)를 보낸다.

```
#ab -n 100 -c 10 127.0.0.1:5000/

Benchmarking 127.0.0.1 (be patient).....done


Server Software:        Werkzeug/0.14.1
Server Hostname:        127.0.0.1
Server Port:            5000

Document Path:          /test
Document Length:        11 bytes

Concurrency Level:      10
Time taken for tests:   100.257 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      16500 bytes
HTML transferred:       1100 bytes
Requests per second:    1.00 [#/sec] (mean)
Time per request:       10025.713 [ms] (mean)
Time per request:       1002.571 [ms] (mean, across all concurrent requests)
Transfer rate:          0.16 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       1
Processing:  1003 9574 1639.4  10025   10029
Waiting:     1002 9574 1639.4  10025   10029
Total:       1003 9574 1639.3  10025   10029

Percentage of the requests served within a certain time (ms)
  50%  10025
  66%  10026
  75%  10026
  80%  10026
  90%  10028
  95%  10028
  98%  10029
  99%  10029
```
tpr이 1개씩 보낼때는 `Time per request: 1002.733 [ms] (mean)` tpr이 10개씩 보낼때는 `Time per request: 10025.713 [ms] (mean)` 10배 가량 증가한 걸 볼 수 있다.
또한 10개씩 보낸다고 해서 request가 실패하지 않고, 동시에 보낸 request는 직렬화되어 한번에 한개씩 처리된다.


## Flask Gunicorn Performance Testing

옵션의 적절한 숫자는 이렇게 계산한다.
cpu =  NUM_CPU
thread = 2 * NUM_CPU + 1

```
#ab -n 100 -c 10 127.0.0.1:5000/

# gunicorn -w 4 --threads 9 app:app

Requests per second:    9.96 [#/sec] (mean)
Time per request:       1003.713 [ms] (mean)
Time per request:       100.371 [ms] (mean, across all concurrent requests)
```

1초에 처리 9.96개의 request를 처리하고, request 10개의 1003.713 ms 걸렸다.

다음은 Sync Workers, Async worker의 성능 비교해보자.

```
# ab -n 10000 -c 1000 127.0.0.1:8000/

gunicorn -w 4 --threads 9 app:app

Requests per second:    35.63 [#/sec] (mean)
Time per request:       28066.789 [ms] (mean)
Time per request:       28.067 [ms] (mean, across all concurrent requests)



gunicorn -k gevent -w 4 --threads 9 app:ap

Requests per second:    891.75 [#/sec] (mean)
Time per request:       1121.395 [ms] (mean)
Time per request:       1.121 [ms] (mean, across all concurrent requests)

```

#### 이미지 인식 테스트

```
# ab -n 100 -c 10  127.0.0.1:8000/predict2


Benchmarking 127.0.0.1 (be patient).....done


Server Software:        gunicorn/19.9.0
Server Hostname:        127.0.0.1
Server Port:            8000

Document Path:          /predict2
Document Length:        3 bytes

Concurrency Level:      10
Time taken for tests:   90.066 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      16200 bytes
HTML transferred:       300 bytes
Requests per second:    1.11 [#/sec] (mean)
Time per request:       9006.590 [ms] (mean)
Time per request:       900.659 [ms] (mean, across all concurrent requests)
Transfer rate:          0.18 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       0
Processing:  3363 8639 4460.7   7225   21624
Waiting:     3363 8638 4460.4   7224   21623
Total:       3363 8639 4460.7   7225   21624

Percentage of the requests served within a certain time (ms)
  50%   7225
  66%  10075
  75%  11911
  80%  12825
  90%  14999
  95%  17420
  98%  18577
  99%  21624


```








<br><br>
#### 참고문헌
* https://matthiaslee.com/performance-testing-101-5-minute-intro/
