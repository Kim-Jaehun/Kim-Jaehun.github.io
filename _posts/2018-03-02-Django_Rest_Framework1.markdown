---
layout: post
date:   2018-03-02 08:43:59
title:  "Django Rest Framework(DRF)"
author: kim-jaehun
categories: [django]
tags: Django Rest Framework
---


#### Django 프로젝트 생성

```
$ sudo pip install django==1.8.3
$ sudo pip install djangorestframework==3.0.4
$ sudo pip install django-rest-swagger
```


```
$ django-admin startproject django_rest_test
$ cd django_rest_test
$ django-admin startapp blog
```

<br>
#### Django DB 생성
```
$ python manage.py migrate
```

<br>
#### 모델

```python
# blog/model.py

from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=256)
    content = models.CharField(max_length=256)
    reg_date = models.DateTimeField(auto_created=True, auto_now=True)
```
모델의 필드를 정의한다. 필드는 데이스베이스에서 컬럼과 매핑된다.

<br>
#### 설정 및 DB 업데이트

```python
#django_rest_test/setting.py

INSTALLED_APPS = (
    'rest_framework_swagger',
    'rest_framework',
    'blog',
)
```

`django-admin startapp blog`로 만들었던 `blog` 앱을 Django에 추가한다.


```
$ python manage.py makemigrations
$ python manage.py migrate
```
`python manage.py makemigrations`모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 migration 으로 저장시키고 싶다는 것을 Django 에게 알려줍니다
migration 은 Django가 모델(즉, 데이터베이스 스키마를 포함한)의 변경사항을 저장하는 방법으로써, 디스크상의 파일로 존재합니다.
`python manage.py migrate` migration 들을 실행시켜주고, 자동으로 데이터베이스 스키마를 관리


<br>
#### DB test
```
$ python manage.py shell
...
>> from blog.models import post
# 데이터 삽입
>> Post.objects.create(title='제목2', content='내용2')
# 데이터 조회
>> Post.objects.all()
```




<br>
#### 뷰

```python
class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post

class blog_api(GenericAPIView, mixins.ListModelMixin):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```
* 클래스 기반 뷰(Class-based view)
* GenericAPIView 를 상속

기본적으로 questset 이라는 멤버와 serializer_class 라는 멤버(또는 get_serializer_class() 라는 함수)를 제공해줘야 합니다. 이를 통해서 GenericAPIView는 기본적인 REST 기능을 수행할 수 있게 됩니다.

Serializer 는 REST 로 데이터를 주고 받을 때, 모델을 어떻게 주고 받을 것인가를 정의하기 위한 클래스 입니다
저희는 데이터 모델 그 자체를 주고 받을 것 이니, 기본적으로 모델 전체를 자동으로 변환해주는 ModelSerializer 에 힘을 빌릴겁니다.

 ModelSerializer 는 Meta 클래스를 요구합니다. 클래스 안에 Meta 클래스를 정의하는 것으로, ModelSerializer가 자신이 필요한 정보들을 파악하여 자동으로 Serialize 를 수행하게 될 것입니다.


<br>
#### Urls

```python
from django.conf.urls import include, url
from django.contrib import admin
from blog.views import blog_api

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),

    #Rest Test
    url(r'^api/blog/', blog_api.as_view()),
]
```
`http:xxx.xxx.xxx.xxx:XXXX/api/blog`로 접속

 as_view() - 기본적인 REST Framework 웹 페이지를 하나 출력해 줍니다

<br>
#### Django 서버 실행 및 테스트

```
$ python manage.py runserver
```
```
http://127.0.0.1:8000/api/blog
```

![django_rest_test_example](https://drive.google.com/uc?id=1ddETwBCYCjFFyfhhRipKn4zoO0Rd-pFS)








<br><br>
#### 참고문헌
* https://devissue.wordpress.com/2015/02/03/pycharm%EA%B3%BC-%ED%95%A8%EA%BB%98-django%EC%99%80-restframework%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%98%EC%97%AC-rest-api-%EB%A7%8C%EB%93%A4%EA%B8%B0/
* https://docs.djangoproject.com/ko/2.1/intro/tutorial02/
