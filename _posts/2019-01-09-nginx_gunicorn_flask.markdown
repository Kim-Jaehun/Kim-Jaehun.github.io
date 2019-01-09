---
layout: post
title:  "Nginx Gunicorn flask"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

###Nginx Gunicorn Flask
`flask` 앱은 단순히 실행하는 것은 테스트용으로 서버로는 적절하지만 서비스에는 부적절하다.

그렇기때문에 `Nginx`<->`Gunicorn` <->`flask` 형태로 구성하여 배포한다.

<br>
### 가상환경 변수 설치 및 설정
```
$ sudo pip3 install virtualenv
```

```
$ mkdir serve_project
$ cd serve_project
$ virtualenv serve_project_env
$ source serve_project_env/bin/activate
```

<br>
### Flask 설치 및 스크립트 작성
```
(serve_project_env) $ pip3 install flask
```
```
(serve_project_env) $ vi flask_test.py
```
``` python
# flask_test.py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "<h1 style='color:blue'>Hello There!</h1>"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```
간단한 flask 앱입니다. `http://localhost:[port]/`를 요청하게 되면 Hello There을 출력하는 예제입니다.

```
(serve_project_env) $ sudo ufw allow 5000
```
5000번 port 열어야 합니다.

<br>
### WSGI Entry Point(Gunicorn) 설치 및 연결

```
(serve_project_env) $ pip3 install gunicorn
```
`gunicorn` 설치
```
(serve_project_env) $ vi wsgi.py
```
```
#wsgi.py
from flask_test import app

if __name__ == "__main__":
    app.run()
```
`flask_test.py`을 임포트해서 실행시기는 파일이다.

```
(serve_project_env) $ gunicorn --bind 0.0.0.0:5000 wsgi:app
```
`gunicorn`을 이용해서 `wsgi.py`를 실행시키고, `http://localhost:5000/`로 접속하게 되면 `Hello There`을 볼 수 있다.

```
(serve_project_env) $ deactivate
```

<br>
### Nginx 설치 및 세팅 변경
```
$ sudo apt-get install nginx
```
```
$ sudo vi /etc/nginx/sites-available/serve_project
```
```
#/etc/nginx/sites-available/serve_project
server {
    listen 5000;
    server_name _;

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/kimjaehun/serve_project/test.sock;
    }
}
```
```
$ sudo ln -s /etc/nginx/sites-available/serve_project /etc/nginx/sites-enabled
```
`nginx`는 5000번 포트로 접속하고, 소켓을 이용해서 `gunicorn`과 통신한다.

```
$ sudo service nginx restart
```
<br>
### Nginx 와 Gunicorn 연결

```
source serve_project_env/bin/activate
```
```
(serve_project_env) $ gunicorn --unix:test.sock wsgi:app
```
`gunicorn`에서도 소켓통신을 하도록 지정한다. `http:localhost:5000`으로 접속하게 되면  `Hello There`을 볼 수 있다.













<br><br>
#### 참고문헌
* https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04
