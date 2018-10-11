---
layout: post
title:  "Android Otto EventBus"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### Otto EventBus란?

`Otto can be used to communicate between your activity and fragments or to communicate between an activity and a service.`

Otto는 (Activity, Fragment) , (Activity, Service) 사이의 의사소통을 위해 사용될 수 있다.

<br>
#### Installation
```
dependencies {
  compile 'com.squareup:otto:1.3.8'
}
```

<br>
#### Bus.class 생성
Otto를 사용하기 위해서, 싱글톤으로 인스턴스를 생성

``` JAVA
public final class BusProvider {

    private static final Bus BUS = new Bus();

    private BusProvider() {
    }
    public static synchronized Bus getInstance()
    {
        return BUS;
    }
}
```

<br>
#### Event 등록 및 해제

activity(fragment)에 등록 및 해제
`@Subscribe` 통해 이벤트를 감지하고 수행할 method를 작성합니다

```JAVA
@Override
protected void onCreate(Bundle arg0) {
    super.onCreate(arg0);
    BusProvider.getInstance().register(this);
}

@Override
protected void onDestroy() {
    BusProvider.getInstance().unregister(this);
    super.onDestroy();

}

// subscribe for string messages
@Subscribe
public void getMessage(String s) {
    Toast.makeText(this, s, Toast.LENGTH_LONG).show();
}

//subscribe for TestData messages

@Subscribe
public void getMessage(TestData data) {
    Toast.makeText(getActivity(), data.message, Toast.LENGTH_LONG).show();
}
```

<br>
#### Event 전달

```JAVA
// post a string object
bus.post("Hello");
```

```JAVA
// example data to post
public class TestData {
    public String message;
}
// post this data
bus.post(new TestData().message="Hello from the activity");
```





<br><br>
#### 참고문헌
* http://www.vogella.com/tutorials/JavaLibrary-EventBusOtto/article.html
