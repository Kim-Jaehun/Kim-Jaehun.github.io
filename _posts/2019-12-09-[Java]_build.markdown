---
layout: post
title:  "[JAVA] build란"
date:   2019-12-09 08:43:59
author: kim-jaehun
categories: java
tags: java
sitemap :
  changefreq : daily
  priority : 1.0
comments: true
---

- Java에서 빌드란 무엇인가를 알아보고, 대표적인 빌드도구를 알아본다.

---


### 빌드(Build)란?

먼저, Java에서 컴파일(compile), 빌드(build)에 대해서 구별해야 한다.

* compile? - 컴파일러에 의해서 .java파일을 .class파일로 변환하는 작업
* budil? - 실행가능한 형태로 만드는 것. build = 컴파일(compile) + 테스트(Test) + 검사(Inspect) + 배포I(Deploy)를 다 합쳐 빌드(build)라고 한다.


`일반적으로 우리가 사용하는 eclipse의 run 버튼이 빌드 + 실행를 의미한다.`

### 빌드 도구(build tool)란?

대표적인 3가지의 빌드 도구

* Ant
* Maven
* Gradle


#### Ant

https://ant.apache.org/bindownload.cgi

이곳에서 버전에 맞게 다운로드한다.

`1.9.1 .tar.gz archive: apache-ant-1.9.1-bin.tar.gz [PGP] [SHA512]` 다운로드했다.

java 버전과의 호환으로 조금 낮은 버전을 사용했다.

압축을 푼 후 bin/ant 파일을 실행하면 된다.

```
./ant

/*
Buildfile: build.xml does not exist!
Build failed
*/
```

`build.xml`이 존재하지 않는다고 나온다.

`build.xml` 작성한다.
```
#build.xml

<project name="test" basedir=".">
     <description>
          Ant test
     </description>
<property name="src" location="src"/>
<property name="build" location="build"/>

<target name="init" description="초기화 영역입니다">
     <delete dir ="${build}"/>
     <mkdir dir="${build}"/>
</target>

<target name="compile" depends="init" description="컴파일하는 영역입니다">
     <javac destdir="${build}" debug="on">
     <src path="${src}"/>
     </javac>
</target>
</project>
```

xml 간단하게 설명하면, `init`, `compile`이라는 target을 설정한다.
* init - 현재 디렉토리에 build/를 생성
* compile - src/에 존재하는 .java파일을 build/에 .class파일로 변환한다.

```
# ./ant init

Buildfile: /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build.xml

init:

   [mkdir] Created dir: /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build
BUILD SUCCESSFUL
Total time: 0 seconds

```

```
#./ant compile
Buildfile: /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build.xml

init:
   [delete] Deleting directory /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build
    [mkdir] Created dir: /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build

compile:
    [javac] /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build.xml:14: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds
    [javac] Compiling 1 source file to /home/kimjaehun/다운로드/apache-ant-1.9.1/bin/build

BUILD SUCCESSFUL
Total time: 1 second
```

#### Maven



#### Gradle







#### 참고문헌
* https://everyflower.tistory.com/141
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzMzNDE4OTg5XX0=
-->