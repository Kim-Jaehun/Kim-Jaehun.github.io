---
layout: post
title:  "다중 DB 연결 in spring"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Spring]
tags: Spring
---


## 다중 DB 연결 in spring

* 웹페이지를 스프링으로 개발 시 여러 개 DB(ex. Mysql, oracle..)를 사용하는 경우가 있다.

* 예제에서는 두개의 MySQL을 연결한다.

<br>
###1. context-datasource.xml 에 dataSource2 추가
```java
<bean id="dataSource1" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
  <property name="driverClassName" value="com.mysql.jdbc.Driver" />
  <property name="url" value="jdbc:mysql://IP1:port/TABLE1" />
  <property name="username" value="****" />
  <property name="password" value="****" />
</bean>

<bean id="dataSource2" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
  <property name="driverClassName" value="com.mysql.jdbc.Driver" />
  <property name="url" value="jdbc:mysql://IP2:port/TABLE2" />
  <property name="username" value="****" />
  <property name="password" value="****" />
</bean>
```
<br>
###2. sqlSession2, sqlSessionTemplate2 를 추가
```java
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />
  <property name="mapperLocations" value="classpath:/mapper/common/common_SQL.xml" />
</bean>
<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
  <constructor-arg index="0" ref="sqlSession" />
</bean>

<bean id="sqlSession2" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource2" />
	<property name="mapperLocations" value="classpath:/mapper/user/user_SQL.xml" />
</bean>
<bean id="sqlSessionTemplate2" class="org.mybatis.spring.SqlSessionTemplate">
	<constructor-arg index="0" ref="sqlSession2" />
</bean>
```

<br>
###3. @Resource을 이용해서 각각의 sqlSessionTemplate 지정

```java
@Autowired
public SqlSession sqlSession;
```

```java
@Autowired
@Resource(name = "sqlSessionTemplate")
public SqlSession sqlSession;

@Autowired
@Resource(name = "sqlSessionTemplate2")
public SqlSession sqlSession2;
```

<br>
<br>
<br>
