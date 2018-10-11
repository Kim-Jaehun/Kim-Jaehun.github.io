---
layout: post
title:  "Android OkHttp"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### OkHttp란?

OkHttp는 HTTP 및 HTTP/2 통신을 보다 쉽게 할 수 있도록 다양한 기능을 제공해주는 Android 및 Java 용 라이브러리입니다.

<br>
#### Installation
```
dependencies {
  compile 'com.squareup.okhttp3:okhttp:3.3.0'
}
```


#### class 생성
Singleton으로 class생성
```Java
Class ConnectServer{
   OkHttpClient client = new OkHttpClient();
   private static HttpConnection instance = new HttpConnection();
   public static HttpConnection getInstance() {
     return instance;
   }
}

```


<br>
#### GET

``` JAVA

OkHttpClient client = new OkHttpClient();

public void requestGet(String url, String searchKey){

    HttpUrl.Builder urlBuilder = HttpUrl.parse(url).newBuilder();
    urlBuilder.addEncodedQueryParameter("searchKey", searchKey);
    String requestUrl = urlBuilder.build().toString();
    Request request = new Request.Builder().url(requestUrl).build();

    client.newCall(request).enqueue(new Callback() {
        @Override
        public void onFailure(Call call, IOException e) {
            Log.d("error", "Connect Server Error is " + e.toString());
        }

        @Override
        public void onResponse(Call call, Response response) throws IOException {
            Log.d("aaaa", "Response Body is " + response.body().string());
        }
    });
}


```

<br>
#### POST
```JAVA
public void requestPost(String url, String id, String password){
            RequestBody requestBody = new FormBody.Builder().add("userId", id).add("userPassword", password).build();
            Request request = new Request.Builder().url(url).post(requestBody).build();
            client.newCall(request).enqueue(new Callback() {
                @Override
                public void onFailure(Call call, IOException e) {
                    Log.d("error", "Connect Server Error is " + e.toString());
                }

                @Override
                public void onResponse(Call call, Response response) throws IOException {
                    Log.d("aaaa", "Response Body is " + response.body().string());
                }
            });
        }

```


#### 사용법
```JAVA
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ConnectServer connectServer = new ConnectServer();
        connectServer.requestGet(url, "searchKey 변수 값");
        connectServer.requestPost(url, "id","pwd");   
    }
```


<br><br>
#### 참고문헌

* http://www.vogella.com/tutorials/JavaLibrary-OkHttp/article.html

* http://onlyformylittlefox.tistory.com/6 [Trace]
