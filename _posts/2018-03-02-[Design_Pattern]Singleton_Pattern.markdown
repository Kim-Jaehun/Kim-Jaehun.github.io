---
layout: post
title:  "Singleton Pattern(싱글톤 패턴)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Design Patten]
tags: Singleton_pattern
---

### Singleton Pattern(싱글톤 패턴) 이란?

최초 한번만 메모리를 할당하고(Static) 그 메모리에 인스턴스를 만들어 사용하는 디자인패턴.


```JAVA
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}

    public static Singleton getInstance() {
         if(uniqueInstance == null) {
              uniqueInstance = new Singleton();
         }
    return uniqueInstance;
    }
    .......
    .......
}
```

###멀티태스킹 문제 해결한 싱글턴 패턴(The Singleton Pattern)

- 처음에 인스턴스 생성
```JAVA
public class Singleton {
    private static Singleton uniqueInstance = new Singleton();
    private Singleton() {}

    public static Singleton getInstance() {    
      return uniqueInstance;
    }
}
```

- DCL (Double-Checking Locking)
```JAVA
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}

    public static Singleton getInstance() {
         if (uniqueInstance == null){
           synchronized (Singleton.class){
             if(uniqueInstance == null) {
                  uniqueInstance = new Singleton();
             }
           }
         }
    return uniqueInstance;
    }
    .......
    .......
}
```


<br><br>
#### 참고문헌
* http://jeong-pro.tistory.com/86
* https://www.androidpub.com/2385823
* http://dreamlog.tistory.com/495
