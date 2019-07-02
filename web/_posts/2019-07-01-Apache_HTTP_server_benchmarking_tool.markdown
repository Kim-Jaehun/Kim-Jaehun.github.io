---
layout: post
title:  "ab(Apache HTTP server benchmarking tool) 아파치 웹서버 성능검사 도구"
date:   2019-06-26 08:43:59
author: kim-jaehun
categories: [web]
tags: [web]
sitemap :
  changefreq : daily
  priority : 1.0
---


## ab - 아파치 웹서버 성능검사 도구

ab는 아파치 하이퍼텍스트 전송 프로토콜 (HTTP) 서버의 성능을 검사하는(benchmarking) 도구이다. 현재 아파치가 어떻게 동작하는지 알려준다. 특히 아파치가 현재 초당 몇개의 요청을 서비스하는지 알려준다.

#### 설치
```
$sudo apt-get install apache2-utils
```

#### 옵션

<table>  
<tbody><tr>  
<th style="width:100px">옵션</th>  
<th>설명</th>  
</tr>  
<tr>  
<td>-n</td>  
<td>성능을 검사하기위해 보내는 요청수. 기본값으로 요청을 한번만 보내기때문에 일반적인 성능검사 결과를 얻을 수 없다.</td>  
</tr>  
<tr>  
<td>-c</td>  
<td>동시에 요청하는 요청수. 기본적으로 한번에 한 요청만을 보낸다.</td>  
</tr>  
<tr>  
<td>-g</td>  
<td>측정한 모든 값을 'gnuplot' 혹은 TSV (Tab separate values, 탭으로 구분한 값) 파일에 기록한다. 라벨은 output 파일의 첫번째 라인을 참고한다.</td>  
</tr>  
<tr>  
<td>-t</td>  
<td>성능을 검사하는 최대 초단위 시간. 내부적으로 -n 50000을 가정한다. 정해진 시간동안 서버 성능을 검사할때 사용한다. 기본적으로 시간제한 없이 검사한다.</td>  
</tr>  
<tr>  
<td>-v</td>  
<td>출력 수준을 지정한다. <br>4 이상이면 헤더에 대한 정보를, <br>3 이상이면 (404, 202, 등) 응답코드를, <br>2 이상이면 경고(warning)와 정보(info)를 출력한다.</td>  
</tr>  
<tr>  
<td>-A</td>  
<td>프록시를 통해 BASIC Authentication 정보를 제공한다. <br>:로 구분한 사용자명과 암호를 <strong>base64 인코딩</strong>하여 전송한다.</td>  
</tr>  
<tr>  
<td>-X</td>  
<td> proxy[:port]    프록시 서버를 사용하여 요청한다.</td>  
</tr>  
</tbody></table>

#### 사용법

```
$ab -n 100 -c 10 127.0.0.1/


This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 127.0.0.1 (be patient).....done


Server Software:        nginx/1.10.3
Server Hostname:        127.0.0.1
Server Port:            80

Document Path:          /
Document Length:        612 bytes

Concurrency Level:      10
Time taken for tests:   0.084 seconds
Complete requests:      100
Failed requests:        0
Total transferred:      85400 bytes
HTML transferred:       61200 bytes
Requests per second:    1190.19 [#/sec] (mean)
Time per request:       8.402 [ms] (mean)
Time per request:       0.840 [ms] (mean, across all concurrent requests)
Transfer rate:          992.60 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.1      0       0
Processing:     0    8  15.8      1      50
Waiting:        0    8  15.8      1      50
Total:          1    8  15.8      1      50

Percentage of the requests served within a certain time (ms)
  50%      1
  66%      1
  75%      1
  80%     27
  90%     49
  95%     49
  98%     50
  99%     50
 100%     50 (longest request)

```


<br><br>
#### 참고문헌

*  https://httpd.apache.org/docs/2.2/ko/programs/ab.html
