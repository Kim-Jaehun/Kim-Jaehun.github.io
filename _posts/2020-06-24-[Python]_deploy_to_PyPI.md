---
layout: post
title:  "[Python] python 라이브러리 배포(to PyPi)"
date:   2020-06-24 08:43:59
author: kim-jaehun
categories: python
tags: python
sitemap :
  changefreq : daily
  priority : 1.0
comments: true
---

- 내가 만든 파이썬 프로그램을 PyPi에 배포

---

# 파이썬 프로그램 배포

자신이 만든 프로그램을 **PyPi**에 배포해보자.

> **PyPi:**  Python Package index이다. 간단히 말해 , 파이썬 패키지 저장소이다.
> 우리는 **pip**를 통해서 PyPi의 패키지를 설치해서 사용 할 수 있다.



## PyPi  가입

[ https://pypi.org/](https://pypi.org/) 에서 가입을 한다.
가입에 대한 내용은 생략.

└── tests  
 ├── __init__.py  
 └── test_array.py

##  나의 파이썬 프로그램 

    # 디렉토리 구조
    Korean
	   └──  korean
		    └── __init__.py
			├── text_processor.py
	    ├──  setup.py
	    ├──  README.md
	    └── requirements.txt
    

> **__init__.py**  파일을 패키지 폴더에 만들어서, 패키지임을 인식시켜주어야 한다.

```python
#setup.py
    
import setuptools

setuptools.setup(
	name="Korean-WikiExtractor",
	version="0.1",
	license='MIT',
	author="Kim Jaehun",
	author_email="wogjs217@gmail.com",
	description="convert MediaWiki to plain text",
	long_description=open('README.md').read(),
	url="https://github.com/Kim-Jaehun/Korean-WikiExtractor",
	packages=setuptools.find_packages(),
	classifiers=[
	    # 패키지에 대한 태그
	    "Programming Language :: Python :: 3.속6",
	],
)
```
setup .py 를 자신에 맞게 작성한다. 

##   패키지 빌드
```
pip install setuptools wheel
```
pip를 통하여 setuptools, wheel 패키지를 설치
```
python setup.py sdist bdist_wheel
```
setup.py를 실행
**sdist** - 소소 배포판을 만드는 옵션
**bdist_wheel** - 순수 휠 또는 플랫폼 휠을 빌드하는 명령

##  패키지 배포
```
python install twine
```
```
 python -m twine upload dist/* 
```
dist/ 폴더 밑에 모든 파일들이 업로드된다.

##  패키지 사용
```
pip install customLib
```
```python
import dfsfs

fdsffdsfsdf
```





<!--stackedit_data:
eyJoaXN0b3J5IjpbODI5MjI2NywtMTU4MTY5MTcxMV19
-->