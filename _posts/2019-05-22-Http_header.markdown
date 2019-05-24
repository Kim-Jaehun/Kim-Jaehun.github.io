---
layout: post
title:  "HTTP 헤더 구조"
date:   2019-05-22 08:43:59
author: kim-jaehun
categories: []
tags:
sitemap :
  changefreq : daily
  priority : 1.0
---

##HTTP header란

```
GET / HTTP/1.1
Host: 127.0.0.1:3000
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate, br
Accept-Language: ko,en;q=0.9,en-US;q=0.8,ko-KR;q=0.7
Cookie: _ga=GA1.1.1449535109.1557886866


HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 746
Server: Werkzeug/0.14.1 Python/3.6.5
Date: Fri, 24 May 2019 07:52:38 GMT
```

<br>
##### Host
* 요청을 한 서버의 Host

##### Connection
* 기본적으로 keep-alive로 되어있는데 사실상 아무런 의미도 없습니다. HTTP/2에서는 아예 사라져버렸습니다.

##### User-Agent
* 현재 사용자가 어떤 클라이언트(운영체제와 브라우저 같은 것)를 이용해 요청을 보냈는지

##### Accept
* 클라이언트 자신이 원하는 미디어 타입 및 우선순위를 알린다.

##### Content-Type

* 요청한 파일의 MIME 타입을 나타냅니다.
* 미디어 타입 (MIME: Multipurpose Internet Mail Extensions)`
* MIME 타입은 클라이언트에 전송된 문서의 형식을 알려주기 위한 메커니즘입니다.

#####`Accept vs Content-Type`
* Request Header의 Accept 는 클라이언트가 어떤 컨텐츠 타입을 받길 원하는가 이고, Content-Type 은 어떤 컨텐츠 타입을 실제로 보내는가 를 기록한다.

##### Cookie
* 웹서버가 클라이언트에 쿠키를 저장해 놓았다면 해당 쿠키의 정보를 이름-값 쌍으로 웹서버에 전송합니다.

등등..



#### 참고문헌
* https://www.zerocho.com/category/HTTP/post/5b3ba2d0b3dabd001b53b9db
* http://1ambda.github.io/content-type-vs-accept-http-header/
* https://gmlwjd9405.github.io/2019/01/28/http-header-types.html
