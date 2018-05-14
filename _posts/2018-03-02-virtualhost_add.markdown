---
layout: post
title:  "가상호스트(서브도메인) 추가"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [WEB]
tags: WEB
---


## 가상 호스트란?
* 하나의 머신에서 여러 개의 호스트를 운용하는 기술
* 서버 1대에서 여러 개의 도메인을 운용하는 기술


## 서브 도메인이란?

<table>
  <thead>
    <tr>
      <th>메인도메인</th>
      <th>서브도메인</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td >example.com</td>
      <td>west.example.com, east.example.com</td>
    </tr>
    <tr>
      <td>wikipedia.org</td>
      <td>ko.wikipedia.org, en.wikipedia.org</td>
    </tr>
  </tbody>
</table>


<br>

## 가상 호스트 추가

### 1. server.xml에 서브 도메인 호스트 추가
서브 도에인 연결 시킬 호스트 추가 (kef.localhost, dev.localhost)

![server.xml](https://drive.google.com/uc?id=10N0j6mLxN0BFyBIPo3FGSkLRXmJh_pSN)


<br>
### 2. 서브도메인명과 동일한 폴더 생성

![dev,kef_directory_add](https://drive.google.com/uc?id=1003WH7s2jZsctthhuUyeSS6U1A15lwFq)

<br>
### 3. ROOT.xml 생성
CATALINA_HOME/conf/Catalina/dev.localhost – ROOT.xml 생성 및 수정
CATALINA_HOME/conf/Catalina/kef.localhost – ROOT.xml 생성 및 수정

![dev,kef_localhost/ROOT.xml](https://drive.google.com/uc?id=14LEL-JnEG4lBSRtp5Ekydsio0emVXK1U)

![ROOT.xml](https://drive.google.com/uc?id=1hJNRWapG3KUrwUbcuFFVUioG7RGapmg9)

<br>
### 4. ROOT.xml 수정  docBase 지정
CATALINA_HOME/conf/Catalina/dev.localhost/ROOT.xml  - docBase에 설정할 폴더(dev.localhost, kef.localhost) 를 CATALINA_HOME 위치에 생성

![ROOT.xml](https://drive.google.com/uc?id=1hJNRWapG3KUrwUbcuFFVUioG7RGapmg9)

![dev,kef_directory_add2](https://drive.google.com/uc?id=17ZVBlKDhdubuhA8NG-3zwNTiH3xlqljW)

<br>
### 5. 결과 확인

이 경우에는 kef.localhost, dev.localhost 에 /maintest라는 프로젝트가 들어있음

![result_check](https://drive.google.com/uc?id=16qCZs__aFN4T5Rc7UFpmubEbIoduFcs1)



<br>
<br>
#### 참고문헌

* https://zetawiki.com/wiki/%EC%84%9C%EB%B8%8C%EB%8F%84%EB%A9%94%EC%9D%B8
* http://thereclub.tistory.com/11
