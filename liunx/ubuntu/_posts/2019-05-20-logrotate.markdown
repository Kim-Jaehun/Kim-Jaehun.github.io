---
layout: post
title:  "Logrotate에 nginx 등록"
date:   2019-05-20 08:43:59
author: kim-jaehun
categories: [liunx, ubuntu]
tags: [liunx, ubuntu]
sitemap :
  changefreq : daily
  priority : 1.0
---

##Logrotate란

logrotate 는 특정 파일을 일정 시점, 압축, 메일보내기 등 로그를 저장하는 명령어이다.


* `cron` 스케줄러에 의해 매일 실행된다.
* `/etc/logrotate.conf` (설정 파일)
* `/etc/logrotate.conf` 에서 `/etc/logrotate.d`를 include 하고 있다. 모든 `/etc/logrotate.d/` 밑의 모든 하위 파일들은 설정파일에 포함된다.
* `/etc/logrotate.d/`밑에 설정파일을 가지고 있는 서비스(Apache webserver, postgreSQL, MySql, KDE desktop manager etc.)는 logrotate를 통해서 로그를 기록한다.

<br><br>
## Logrotate에 nginx 등록

```
# vi /etc/logrotate.d/nginx access-log

/apps/apps/test/fail_exposition/log/access{
        daily
        rotate 30
        dateext
        dateformat .%Y-%m-%d.log
        missingok
        compress
        delaycompress
        notifempty
        sharedscripts
        postrotate
                [ -f /apps/server/nginx/logs/nginx.pid ] && kill -USR1 `cat /apps/server/nginx/logs/nginx.pid`
        endscript
}

```

`/etc/logrotate.d/nginx access-log`에 파일을 생성한다.

"kill -USR1 cat /apps/server/nginx/logs/nginx.pid". This does not kill the Nginx process, but instead sends it a signal causing it to reload its log files.

실제로 `nginx` 프로세스를 죽이지 않고, log files 를 reload를 유발시키는 시그널을 보낸다.

<br><br>
### Logrotate 옵션에 대한 설명


create 0644 nginx nginx : 생성된 파일은 0644권한을 주며 nginx유저와 nginx그룹을 가지도록 한다.<br>
daily : 일 단위로 rotate한다.<br>
rotate 30 : 30개 이상된 로그파일을 삭제<br>
missingok : 로그파일이 없더라도 오류를 발생시키지 않는다.<br>
notifempty : 파일의 내용이 없을 경우 파일을 생성하지 않는다.<br>
dateext : 1,2,3... 같은 라벨이 아닌 날짜를 붙여서 파일을 생성한다.<br>
dateformat .%Y-%m-%d.log : 파일에 .YYYY-MM-DD.log를 뒤에 붙인다. dateexe옵션과 같이 쓰인다.<br>
sharedscripts : postrotate ~ endscript안에 script가 동작할때 각각의 로그마다 실행 되는 것이 아니라 1번만 실행되도록 하는 옵션이다. (즉) `/home/user/log/nginx/*.log{ ... }` 으로 확장자가 log라는 파일을 rotate할때 각각의 파일이 아닌 1번만 postrotate ~ endscript안의 스크립트를 실행하도록 하는 옵션이다.<br>
maxage 30 : 30일 이상된 로그파일 삭제<br>
size 용량 : 지정된 용량이 되면 rotate한다.<br>
ifempty : 로그파일이 비어있는 경우에도 rotate한다.<br>
monthly : 월단위로 rotate한다.<br>
weekly : 주단위로 rotate한다.<br>
compress : gzip으로 압축한다.<br>
nocompress : 압축하지 않는다.<br>
mail 메일주소 : 설정에 의해 보관 기간이 끝난 파일을 메일주소로 발송한다.<br>
mailfirst 메일주소 : 원본파일을 메일주소로 보낸다.<br>
nomail : 메일을 받지 않는다.<br>
errors 메일주소 : rotate중 오류발생시 메일주소로 통보한다.<br>
copytruncate : 로그파일의 내용을 복사하고 로그파일의 크기를 0으로 만들도록 동작하게 하는 옵션. (만약 파일용량이 크다면 하지 복사하는데 I/O자원이 소모되므로 피해야 한다)<br>



<br><br>
#### 참고문헌
* https://linuxconfig.org/logrotate
* https://www.digitalocean.com/community/tutorials/how-to-configure-logging-and-log-rotation-in-nginx-on-an-ubuntu-vps
*  https://devnerd.tistory.com/entry/1-Nginx-log를-logrotate를-사용하여-일자별로-관리하기
