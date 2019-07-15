---
layout: post
title:  "RabbitMQ_MultiThreads_with_Mysql"
date:   2019-07-15 08:43:59
author: kim-jaehun
categories: []
tags:
sitemap :
  changefreq : daily
  priority : 1.0
---

##RabbitMQ MultiThreads with Mysql

RabbitMQ의 Comsumer를 MultiThreads로 만들고, 각 스레드에서 Mysql 데이터 조회


```python
import pika
import logging
import threading
import requests
import json
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base


from sqlalchemy import or_, and_, text

QUEUE_NAME = 'hello'
RABBITMQ_URL = 'localhost'
MYSQL_URL = 'mysql://USERNAME:PASSWORD@IP/TABLE?charset=utf8



engine = create_engine(MYSQL_URL, echo=True)
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))

Base = declarative_base()


class WorkerThread(threading.Thread):
	def __init__(self, threadID, name, counter):
		threading.Thread.__init__(self)
		self.threadID = threadID
		self.name = name
		self.counter = counter


	def initRabbitMQ(self):
		self.q_conn = pika.BlockingConnection(pika.ConnectionParameters(host=RABBITMQ_URL))
		self.q_channel = self.q_conn.channel()
		self.q_channel.queue_declare(queue=QUEUE_NAME)  # create queue


	def initMysql(self):
		self.session = db_session()
		print(self.session)


	def callback(self, ch, method, properties, body):
		print(self.name, '===', body)

    # 각자에 맞게 쿼리

    nutrient_ingredient = self.session.query(Basic.calorific_quantity, Basic.carbohydrate, Basic.protein, Basic.fat) \
			.from_statement(text(
			"SELECT calorific_quantity, carbohydrate, protein, fat  FROM food2.food_basic where food_name=:name")).params(
			name='간장게장').all()

		print("nutrient_ingredient=", nutrient_ingredient)

    # TO do something

	def run(self):
		self.initRabbitMQ()
		self.initMysql()
		self.q_channel.basic_consume(QUEUE_NAME, self.callback, True)
		self.q_channel.start_consuming()
		db_session.remove()


def main():

	max_thread = 3
	threadList = []
	for i in range(max_thread):
		t_name = "WorkerThread-" + str(i)
		t = WorkerThread(i, t_name, i)
		t.start()
		threadList.append(t)

if __name__ == '__main__':
	main()



from sqlalchemy import Table, Column, Integer, String, DateTime, ForeignKey, MetaData, Text, Float
from sqlalchemy.sql import select
from sqlalchemy.orm import mapper


class Basic(Base):
    __tablename__ = "food_basic"
    __table_args__ = {'mysql_collate': 'utf8'}
    number = Column(Integer, nullable=False, primary_key=True)
    food = Column(String(100), nullable=False, default='')
    food_image = Column(String(255), nullable=False, default='')
    food_name = Column(String(100), nullable=False, default='')
    one_time_supply = Column(Float, nullable=False, default='')
    calorific_quantity = Column(Float, nullable=False, default='')
    carbohydrate= Column(Float, nullable=False, default='')
    protein= Column(Float, nullable=False, default='')
    fat= Column(Float, nullable=False, default='')
    sugar_steam= Column(Float, nullable=False, default='')
    sodium= Column(Float, nullable=False, default='')
    cholesterol= Column(Float, nullable=False, default='')
    saturated_fatty_acid = Column(Float, nullable=False, default='')
    trans_fatty_acid= Column(Float, nullable=False, default='')
    year= Column(String(5), nullable=False, default='')

    def as_dict(self):
        return {c.name: getattr(self, c.name) if getattr(self, c.name) is not None else '' for c in self.__table__.columns}


```




<br><br>
#### 참고문헌
* https://bcho.tistory.com/843
