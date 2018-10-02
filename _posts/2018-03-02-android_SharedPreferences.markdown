---
layout: post
title:  "Android SharedPreferences"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: ButterKnife
---

### SharedPreferences란?

안드로이드상에서는 데이터를 저장하기 위해서 다양한 방법을 사용할 수 있습니다. 그중에서도 가장 손쉽게 데이터를 보관할 수 있는 방법으로는 SharedPreferences를 이용하는 방법이 있습니다.


<br>
#### 저장하기
```java
private void saveScore() {
    SharedPreferences pref = getSharedPreferences("Game", Activity.MODE_PRIVATE);
    SharedPreferences.Editor editor = pref.edit();
    editor.putInt("score", 10000);
    editor.commit();
}
```
####불러오기
```java
private void loadScore() {
    SharedPreferences pref = getSharedPreferences("Game", Activity.MODE_PRIVATE);
    int score = pref.getInt("score", 0);
    Toast.makeText(MainActivity.this, "score : " + score, Toast.LENGTH_LONG).show();
}
```


#### 하나의 SharedPreferences를 이용하기 위한 사용법
```java
private AppPref(Context context) {
    sharedPreferences = PreferenceManager.getDefaultSharedPreferences(context);
    this.context = context;
}
```


```
Static SharedPreferences PreferenceManager.getDefaultSharedPreferences(Context)

Parameter:

Context: 사용하기 원하는 SharedPreferences 인터페이스를 포함하는 Context 지정
Return:

SharedPreferences: 인자로 지정된 Context (app 구동 환경)가 기본으로 생성하는 Preference 데이터 파일에 대한 SharedPreferences를 리턴
```
이 메서드는 App level의 기본 Preference 데이터 파일을 생성/사용하는데 사용된다. 예를 들어 com.holim.test.aaa 라는 패키지에 포함되는 A Activity에서 본 메소드를 호출 하면 Preference 프레임웍은 /data/data/패키지이름/shared_pref/com.holim.test.aaa_preferences.xml 이라는 Preference 데이터 파일을 생성한다. 전달된 Context 인자에 따라 패키지이름_preferences.xml 형태로 생성/사용할 Preference 데이터 파일의 이름이 시스템에 의해 자동으로 지정되게 되기 때문에, 호출되는 위치가 어디이든 같은 Context를 인자로 전달해 호출한 getDefaultSharedPreferences(…)메서드는 항상 같은 SharedPreferences 인스턴스를 리턴한다.



#### 참고문헌

* http://theeye.pe.kr/archives/1281

* http://swalloow.tistory.com/59 [MyCloud]

* http://blog.naver.com/PostView.nhn?blogId=wjdtjdgus956&logNo=120119123685&viewDate=&currentPage=1&listtype=0
