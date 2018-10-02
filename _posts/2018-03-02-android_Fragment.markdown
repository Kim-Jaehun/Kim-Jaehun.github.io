---
layout: post
title:  "Fragment"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### Fragment이란?

프래그먼트 (fragment) 는 하나의 액티비티가 여러 개의 화면을 가지도록 만들기위해 고안된 개념입니다.

```
프래그먼트는 자체 수명 주기를 가지고, 자체 입력 이벤트를 받으며,
액티비티 실행 중에 추가 및 제거가 가능한 액티비티의 모듈식 섹션이라고 생각하면 됩니다
(다른 액티비티에 재사용할 수 있는 "하위 액티비티"와 같은 개념)
```


![example_pic](https://drive.google.com/uc?id=1DgHnO7Ut4GkL8WLpeFcfyr7AceiOlGzR)

위의 하나의 액티비티 안에 Fragment_A, Fragment_B or Fragment_C 가 들어있는 경우


<br><br>
#### Fragment 생성 방법
##### 1. 정적인 방법(Layout 리소스 XML을 사용)


``` xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout...
    <fragment
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/fragmentA"
        android:name="com.recipes4dev.examples.fragmentexample1.FragmentA"
        android:layout_weight="1"
        tools:layout="@layout/activity_main" />

</LinearLayout>
```
activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFAAAA">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:text="Fragment A"
        android:gravity="center" />
</LinearLayout>
```
fragment_A.xml
```java
import android.os.Bundle;
import android.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class FragmentA extends Fragment {
    public FragmentA() {
        // Required empty public constructor
    }
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_a, container, false);
    }
}
```
fragment_A.java



<br><br>
##### 2. 동적인 방법(Java 코드에서 FragmentManager를 사용하여 ViewGroup에 지정)


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout...
    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:id="@+id/fragmentBorC" />

</LinearLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#AAFFAA">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:text="Fragment B"
        android:gravity="center" />

</LinearLayout>
```
fragment_b.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#AAAAFF">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="30sp"
        android:text="Fragment C"
        android:gravity="center" />

</LinearLayout>
```
"fragment_c.xml

```JAVA
import android.os.Bundle;
import android.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class FragmentB extends Fragment {
    public FragmentB() {
        // Required empty public constructor
    }
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_b, container, false);
    }
}
```
fragmentB.java
```JAVA
import android.os.Bundle;
import android.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class FragmentC extends Fragment {
    public FragmentC() {
        // Required empty public constructor
    }
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_c, container, false);
    }
}
```
fragmentC.java
```Java
public class MainActivity extends AppCompatActivity {

    private boolean isFragmentB = true ;

    // ... 코드 계속
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ... 코드 계속 [STE]

        Button button1 = (Button) findViewById(R.id.button1) ;
        button1.setOnClickListener(new Button.OnClickListener() {
            @Override
            public void onClick(View v) {
                switchFragment() ;
            }
        });
    }

    public void switchFragment() {
        Fragment fr;

        if (isFragmentB) {
            fr = new FragmentB() ;
        } else {
            fr = new FragmentC() ;
        }

        isFragmentB = (isFragmentB) ? false : true ;

        FragmentManager fm = getFragmentManager();
        FragmentTransaction fragmentTransaction = fm.beginTransaction();
        fragmentTransaction.replace(R.id.fragmentBorC, fr);
        fragmentTransaction.commit();
    }
}
```
MainActivity.java




#### 참고문헌


* http://recipes4dev.tistory.com/m/58
* http://webnautes.tistory.com/1089
