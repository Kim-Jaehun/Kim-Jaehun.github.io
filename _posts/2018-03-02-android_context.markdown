---
layout: post
title:  "Android Context"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### Context란?

```
안드로이드에서는 UI 구성 요소인 뷰에 대한 정보를 손쉽게 확인하거나 설정할 수 있도록 뷰의 생성자에 컨텍스트(Context) 객체를 전달하도록 되어 있습니다.
```

Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.

어플리케이션 환경에 관한 글로벌 정보를 접근하기 위한 인터페이스. Abstract 클래스이며 실재 구현은 안드로이드 시스템에 의해 제공된다. Context 를 통해, 어플리케이션에 특화된 리소스나 클래스에 접근할 수 있을 뿐만 아니라, 추가적으로, 어플리케이션 레벨의 작업 - Activity 실행, Intent 브로드캐스팅, Intent 수신 등, 을 수행하기 위한 API 를 호출 할 수도 있다.



<b>Context  는 크게 두 가지 역할을 수행하는 Abstract 클래스 입니다.</b>
* 어플리케이션에 관하여 시스템이 관리하고 있는 정보에 접근하기
* 안드로이드 시스템 서비스에서 제공하는 API 를 호출 할 수 있는 기능




<br><br>
#### 참고문헌
*http://arabiannight.tistory.com/entry/272 [아라비안나이트]
