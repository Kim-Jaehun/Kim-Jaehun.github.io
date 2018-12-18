---
layout: post
title:  "python correlation(상관 관계)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---


### Django 프로젝트 생성

```
$ django-admin startproject mysite
```
<br><br>
### Django 프로젝트 구조
```xml
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

<table>
  <thead>
    <tr>
      <th>파일명</th>
      <th>파일 위치</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>mysite</td>
      <td>~/</td>
      <td>디렉터리를 담고 있는 단순한 컨테이너</td>
    </tr>
    <tr>
      <td>manage.py</td>
      <td>mysite</td>
      <td>장고 프로젝트와 다양한 방법으로 커뮤니케이션 할 수 있는 커맨드 라인 유틸리티</td>
    </tr>
    <tr>
      <td>mysite</td>
      <td>mysite/mysite</td>
      <td>실제 프로젝트의 파이썬 패키지 입니다. <br>디렉터리의 이름이 파이썬 코드를 임포트할 때 사용할 실제 파이썬 패키지 이름 입니다 (e.g. mysite.urls).</td>
    </tr>
    <tr>
      <td>__init__.py</td>
      <td>mysite/__init__.py</td>
      <td>아무것도 들어 있지 않은 빈 파일이며 파이썬 에게 현재 디렉터리가 파이썬 패키지임을 알려 줍니다</td>
    </tr>
    <tr>
      <td>settings.py</td>
      <td>mysite/settings.py</td>
      <td>장고 프로젝트의 셋팅과 설정이 포함된 파일</td>
    </tr>
    <tr>
      <td>urls.py</td>
      <td>mysite/urls.py</td>
      <td>장고 프로젝트 안의 URL을 선언하는 곳 입니다.<br> 장고 사이트의 컨텐츠 목록입니다.</td>
    </tr>
    <tr>
      <td>wsgi.py</td>
      <td>mysite/wsgi.py</td>
      <td>WSGI 프로토콜을 사용하는 웹서버가 프로젝트의 페이지를 보여주기 위하여 가장 먼저 사용하는 파일</td>
    </tr>
  </tbody>
</table>

<br><br>
### Django 서버 실행
```
$ python manage.py runserver

/* $ python manage.py runserver 8080 */ #포트(8080) 변경해서 실행
/* $ python manage.py runserver 0.0.0.0:8000 */ #IP(0.0.0.0:8000) 변경해서 실행
```

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

August 19, 2016 - 15:50:53
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

<br><br>
#### runserver의 자동 리로딩 기능

>개발용 서버는 필요할 때 마다 자동으로 파이썬 코드를 리로딩 합니다. 코드를 변경한 후에 변경된 내용을 적용하기 위해 서버를 재기동 하지 않아도 됩니다.

>새로운 파일을 추가했을 때는 서버가 자동으로 재기동 하지 않습니다. 이런 경우에는 수동으로 재기동할 필요가 있습니다.


<br><br>
### APP 생성
```
$ python manage.py startapp polls
```

### APP 구조
```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```


###### Projects vs. apps

>앱이란 어떠한 기능을 하는 웹 어플리케이션을 말 합니다. 프로젝트는 여러개의 앱을 포함할 수 있으며, 하나의 앱은 여러개의 프로젝트에 포함 될 수 있습니다


<br><br>
### View 만들기


`poll/views.py`
```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

`poll/urls.py 파일 추가`
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```

`mysite/urls.py`
```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    url(r'^admin/', admin.site.urls),
]
```
1. `poll/views.py` 뷰를 사용하기 위해서는 URLconf를 만들어 뷰를 URL에 맵핑 해야 합니다.

> URLconf는 장고에서 URL과 일치하는 뷰를 찾기 위한 패턴들의 집합입니다.

2. polls 디렉터리안에 URLconf를 만들기 위해서 `urls.py`라는 이름의 파일을 만듭니다.
3. `urls.py`에 URL 패턴을 추가(views파일의 index 함수를 매핑)
> 정규표현식은 ^에서 시작해 $로 끝나는 지를 매칭

4. include() 함수는 루트 URLconf가 다른 URLconf를 참조할 수 있도록 해줍니다
>  $ 가지고 있지 않고, 대신에 문자열 마지막에 / 를 가지고 있습니다.

> include() 를 사용하는 이유는 URL을 간단히 plug-and-play 할 수 있도록 하기 위함입니다. 투표 앱은 자신의 URLconf인 polls/urls.py안에 자신만의 URL을 가지고 있기 때문에 “/polls/” 이나 “/fun_polls/” 또는 “/content/polls/” 등 관리자가 원하는 어떤 root path로도 설정할 수가 있습니다.


<br><br>
###Model

모델은 데이터를 데이터베이스에 저장하기 위한 데이터의 기초입니다. 모델은 저장할 데이터의 중요한 필드와 데이터 형식등을 가집니다.

모델은 마이그레션 파일을 포함한다. Ruby On Rails와는 다르게 모델 파일을 통해 마이그레이션 파일을 만듭니다. 기본적으로 이 마이그레이션 파일은 데이터베이스를 모델에
 맞춰 업데트를 하기 위한 기록 정도라고 볼 수 있습니다.

<br>
```
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```


<br><br>
###데이터베이스에 모델을 위한 테이블 만들기

`mysite/settings.`

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls'
]
```

```
$python3 manage.py makemigrations polls
```
마이그레이션 파일(migration file)로 저장
```
$python3 manage.py migrate polls
```
데이터베이스 스키마 생성었습니다.












<br><br>
#### 참고문헌
