---
layout: post
title:  "SQLAlchemy 시작하기"
date:   2019-05-21 08:43:59
author: kim-jaehun
categories: []
tags:
sitemap :
  changefreq : daily
  priority : 1.0
---

##SQLAlchemy란

* SQL을 수동적으로 쓰는 것을 방지하고 테이블들을 마치 파이썬 객체인 것처럼 사용하기 위해 사용하는 Mapper.
* 파이썬 커뮤니티에서 가장 유명한 ORM 이다. `Flask` 사용자들이 `Sqlalchemy`을 `ORM(Object-relational mapping)` 으로 주로 사용한다.
* `SQLAlchemy` 객체 관계형 매퍼는 데이터베이스 테이블을 이용해 사용자가 정의한 파이썬 클래스의 메소드와 각각의 행을 나타내는 인스턴스로 표현된다.


<br><br>
### Declarative 방식
* 테이블과 모델을 한번에 정의 할 수 있다.
``` python

#database.py

from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy.ext.declarative import declarative_base

engine = create_engine('sqlite:////tmp/test.db', convert_unicode=True)
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))
Base = declarative_base()
Base.query = db_session.query_property()

def init_db():
    # import all modules here that might define models so that
    # they will be registered properly on the metadata.  Otherwise
    # you will have to import them first before calling init_db()
    import models
    Base.metadata.create_all(bind=engine)
```

```python
#models.py

from sqlalchemy import Column, Integer, String
from database import Base

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True)
    email = Column(String(120), unique=True)

    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email

    def __repr__(self):
        return '<User %r>' % (self.name)
```
``` python
# main.py

from database import init_db
init_db()

from database import db_session
from models import User
u = User('admin', 'admin@localhost')
db_session.add(u)
db_session.commit()

User.query.all()
User.query.filter(User.name == 'admin').first()
```

<br>
```python
@app.teardown_appcontext
def shutdown_session(exception=None):
    db_session.remove()
```
Flask will automatically remove database sessions at the end of the request or when the application shuts down:

`Flask`는 자동적으로 `DB Sesseion`을 리퀘스트에 끝나거나 또는 어플리케이션이 종료될때 제거한다.


<br><br>
##Manual  Object Relational Mapping
* 테이블과 클래스를 따로 정의하고 함께 매핑한다는 것입니다.

```Python
#database.py

from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import scoped_session, sessionmaker

engine = create_engine('sqlite:////tmp/test.db', convert_unicode=True)
metadata = MetaData()
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))
def init_db():
    metadata.create_all(bind=engine)
```

```python

#models.py
from sqlalchemy import Table, Column, Integer, String
from sqlalchemy.orm import mapper
from database import metadata, db_session

class User(object):
    query = db_session.query_property()

    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email

    def __repr__(self):
        return '<User %r>' % (self.name)

users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String(50), unique=True),
    Column('email', String(120), unique=True)
)
mapper(User, users)
```




<br><br>
#### 참고문헌
* https://edykim.com/ko/post/getting-started-with-sqlalchemy-part-1/
* http://flask.pocoo.org/docs/1.0/patterns/sqlalchemy/
