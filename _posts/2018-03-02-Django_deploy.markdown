---
layout: post
title:  "Nginx uWSGI Django 배포하기"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---


### virtualenv 설치 및 활성화

```
$ sudo pip3 install virtualenv
```

```
$ virtualenv uwsgi-tutorial
$ cd uwsgi-tutorial
$ source bin/activate
```

<br>
### Django 설치 및 프로젝트 생성
```
(uwsgi-tutorial)$ pip3 install django=1.8.7
```
```
(uwsgi-tutorial)$ django-admin.py startproject testproject
```

testproject라는 이름의 Djano프로젝트가 생성되었다.

<br>
### uWSGI 설치 및 uWSGI python 연동

```
(uwsgi-tutorial)$ sudo vi test.py
```

```python
# test.py
application(env, start_response):
  start_response('200 OK', [('Content-Type','text/html')])
  return [b"Hello World"]
```

```
(uwsgi-tutorial)$ uwsgi --http :8000 --wsgi-file test.py

```
![pic1](https://drive.google.com/uc?id=1Lm6QvKetfWF6LExIbQnklo47-3ZcMRyV)



<br>
### uWSGI Django 연동

```
(uwsgi-tutorial)$ cd testproject
(uwsgi-tutorial)$ uwsgi --http :8000 --module testproject.wsgi
```
![pic2](https://drive.google.com/uc?id=1oqHZllWIqpksFtndbh1b017CgcfwyeBx)



<br>
### Nginx 설치 및 Nginx uWSGI 연동

```
(uwsgi-tutorial)$ sudo apt-get install nginx
```

```
(uwsgi-tutorial)$ sudo vi /etc/nginx/sites-available/testproject_nginx.conf
```
```
# testproject_nginx.conf
# the upstream component nginx needs to connect to
upstream django {
    #server unix:///tmp/testproject.sock; # for a file socket
    server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}
# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name _; # substitute your machine's IP address or FQDN
    charset     utf-8;
    # max upload size
    client_max_body_size 75M;   # adjust to taste
    # Django media
    location /media  {
        alias /home/kimjaehun/uwsgi-tutorial/testproject/media;  # your Django project's media files - amend as required
    }
    location /static {
        alias /home/kimjaehun/uwsgi-tutorial/testproject/static; # your Django project's static files - amend as required
    }
    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /home/kimjaehun/uwsgi-tutorial/testproject/uwsgi_params; # the uwsgi_params file you installed
    }
}
```
```
(uwsgi-tutorial)$ sudo ln -s /etc/nginx/sites-available/testproject_nginx.conf /etc/nginx/sites-enabled/
```

* sites-available : 가상 서버 환경들에 대한 설정 파일들이 위치하는 부분입니다. 가상 서버를 사용하거나 사용하지 않던간에 그에 대한 설정 파일들이 위치하는 곳입니다.
* sites-enabled : sites-available에 있는 가상 서버 파일들중에서 실행시키고 싶은 파일을 symlink로 연결한 폴더입니다. 실제로 이 폴더에 위치한 가상서버 환경 파일들을 읽어서 서버를 세팅합니다.
* nginx.conf : Nginx에 관한 설정파일로 Nginx 설정에 관한 블록들이 작성되어 있으며 이 파일에서 sites-enabled 폴더에 있는 파일들을 가져옵니다
<br>
```
(uwsgi-tutorial)$ sudo vi ~/uwsgi-tutorial/testproject/testproject/settings
```

```
...
...
STATIC_ROOT = os.path.join(BASE_DIR, "static/") //맨밑에 추가
```
```
(uwsgi-tutorial)$ python3 manage.py collectstatic
```
static 폴더가 생성됩니다.

```
(uwsgi-tutorial)$ sudo vi uwsgi_params
```
```
#uwsgi_params

uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
```


```
(uwsgi-tutorial)$ sudo service nginx restart
(uwsgi-tutorial)$ cd ~/uwsgi-tutorial
(uwsgi-tutorial)$ uwsgi --socket :8001 --wsgi-file test.py
```

![pic3](https://drive.google.com/uc?id=1eojrPe0qqJWMZygZinPnxrUL9nZEcZwZ)

<br>
### Unix socket을 이용한 Nginx uWSGI 연동

```
(uwsgi-tutorial)$ sudo vi /etc/nginx/sites-available/testproject_nginx.conf
```
```
# testproject_nginx. conf

server unix:///tmp/testproject.sock; # for a file socket
# server 127.0.0.1:8001; # for a web port socket (we'll use this first)
```
```
(uwsgi-tutorial)$ sudo service nginx restart
(uwsgi-tutorial)$ uwsgi --socket /tmp/testproject.sock --wsgi-file test.py --chmod-socket=666
```
![pic4](https://drive.google.com/uc?id=1l_AkBUi3glKbiN3-XNvkKRpWe-liEvNO)

<br>
### Unix socket을 이용한 Nginx uWSGI 연동

```
(uwsgi-tutorial)$ cd ~/uwsgi-tutorial/testproject
(uwsgi-tutorial)$ uwsgi --socket /tmp/testproject.sock --module testproject.wsgi --chmod-socket=666
```
![pic5](https://drive.google.com/uc?id=1I3VmTr16VT4jrhi_ThhoN6XSVbiXIUTS)

<br>
### .ini 파일을 사용하여 uWSGI 실행

```
(uwsgi-tutorial)$ vi ~/uwsgi-tutorial/testproject/testproject_uwsgi.ini
```

```
#testproject_uwsgi.ini

[uwsgi]
# Django-related settings
# the base directory (full path)
chdir           = /home/kimjaehun/uwsgi-tutorial/testproject/
# Django's wsgi file
module          = testproject.wsgi
# the virtualenv (full path)
home            = /home/kimjaehun/uwsgi-tutorial
# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 1
# the socket (use the full path to be safe
socket          = /tmp/testproject.sock
# ... with appropriate permissions - may be needed
 chmod-socket    = 666
# clear environment on exit
vacuum          = true
```

```
(uwsgi-tutorial)$ uwsgi --ini testproject_uwsgi.ini
```

<br>
### 시스템 전체에 uWSGI 설치
```
(uwsgi-tutorial)$ deactivate
$ sudo pip3 install uwsgi
```



























<br><br>
#### 참고문헌
* http://brownbears.tistory.com/16
