---
layout: post
title:  "Android XML include"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### include란?

#### Re-using layouts with <include/>
Although Android offers a variety of widgets to provide small and re-usable interactive elements, you might also need to re-use larger components that require a special layout.

간단하게 설명하면 중복되는 위젯을 중복해서 사용하지 않고 레이아웃 처럼(html의 top, footer, right 등)과 같이 사용.

<b><include_top.xml></b>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="horizontal" >
    <Button
        android:id="@+id/btn_01"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="wrap_content"
        android:text="버튼1"
        />
    <Button
        android:id="@+id/btn_02"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="wrap_content"
        android:text="버튼2"  
        />
    <Button
        android:id="@+id/btn_03"
        android:layout_width="0dip"
        android:layout_weight="1"  
        android:layout_height="wrap_content"
        android:text="버튼3"
        />
</LinearLayout>
```
<b><activity_main.xml></b>
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <include
        android:id="@+id/top"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        layout="@layout/include_top"
        />

</RelativeLayout>
```

<br><br>
#### 참고문헌
* https://developer.android.com/training/improving-layouts/reusing-layouts
* http://arabiannight.tistory.com/entry/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9CAndroid
