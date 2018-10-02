---
layout: post
title:  "ButterKnife"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: ButterKnife
---

### ButterKnife이란?
Annotate fields with @BindView and a view ID for Butter Knife to find and automatically cast the corresponding view in your layout.

간단히 설명하면 spring의 어노테이션과 비슷한 역활이다.
```xml
<TextView
android:id="@+id/tv_main_title"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:gravity="center"
android:text="TextView!"
android:textSize="20sp"
/>

<Button android:id="@+id/btn_main_intent"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_gravity="center"
android:text="Button!"
/>
```

#### 기존의 방법
``` JAVA
public class MainActivity extends Activity {

    TextView tvTitle;
    Button btnIntent;

    @Override
    void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceSTate);
        setContentView(R.layout.activity_main);

        tvTitle = ((TextView)findViewById(R.id.tv_main_title));
        btnIntent = ((Button) findViewById(R.id.btn_main_intent));

        btnIntent.setOnClickListener(new View.OnClickListener(){
            @Override
            public void onCreate(View view){
                Log.d(TAG, "clicked!");
            }
        });
    }

}
```
<br><br>
#### ButterKnife 사용
``` JAVA
public class MainActivity extends Activity {

    @BindView(R.id.tv_main_title)
    TextView tvTitle;

    @BindView(R.id.btn_main_intent)
    Button btnIntent;

    @Override
    void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceSTate);
        setContentView(R.layout.activity_main);

        ButterKnife.bind(this);
    }

    @OnClick(R.id.btn_main_intent)
    void onIntentButtonClick(){
        Log.d(TAG, "clicked!");
    }

}
```
ButterKnife.bind(this); 메서드로 한번에 바인딩

<br><br>
#### gradle추가
``` JAVA
dependencies {
  compile "com.jakewharton:butterknife:8.8.1"
  annotationProcessor "com.jakewharton:butterknife-compiler:8.8.1"
}
```

#### 참고문헌


* https://beomi.github.io/2017/02/27/HowToMakeWebCrawler-With-Selenium/

* https://calyfactory.github.io/view-binding/
