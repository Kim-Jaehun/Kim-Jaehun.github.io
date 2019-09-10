---
layout: post
title:  "[Linux] curl 사용법"
date:   2019-09-09 08:43:59
author: kim-jaehun
categories: linux
tags: linux
sitemap :
  changefreq : daily
  priority : 1.0
comments: true
---

- curl 사용법에 대해서 알아본다.

---

## curl이란?

- curl 은 command line 용 data transfer tool 이다.

- 간단하게, 명령어를 이용해서 url 요청을 보낼 수 있다.


<br>
###  get 방식, post 방식

```
# get 방식
$ curl http://localhost:8080

# post 방식
$ curl -X post -d 'test=ok&test2=ok'
```

<br>
### post 방식 JSON 데이터 전송

```
$ curl -X  post -d '{"instances": [2.0]}'
```

<br>
#### 옵션
<table>
  <thead>
    <tr>
      <th>옵션</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>-H, --header</td>
      <td>헤더를 보냄</td>
    </tr>
    <tr>
      <td>-i, --include</td>
      <td>Include protocol headers in the output (H/F)</td>
    </tr>
    <tr>
      <td>-I, --head</td>
      <td>Show document info only</td>
    </tr>
    <tr>
      <td>-k, --insecure</td>
      <td>Allow connections to SSL sites without certs (H)</td>
    </tr>    
    <tr>
      <td>-s, --silent</td>
      <td>진행 상태, 에러 메시지등을 보여주지 않음</td>
    </tr>
    <tr>
      <td>-S, --show-error</td>
      <td>-s와 함께 사용되며 실패 시 에러메시지 출력</td>
    </tr>
    <tr>
      <td>-L, --location</td>
      <td>요청페이지가 다른 위치로 옮겨 졌을 경우 새로운 페이지로 다시 재요청</td>
    </tr>
    <tr>
    <td>-X, --request</td>
    <td>HTTP 메소드를 설정 할 수 있음</td>
    </tr>
    <tr>
      <td>-d, --data</td>
      <td>HTTP Post data</td>
    </tr>


  </tbody>
</table>

<br><br>
#### 참고문헌
* https://www.lesstif.com/pages/viewpage.action?pageId=14745703
