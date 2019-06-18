---
layout: post
title:  "Protocol Buffers"
date:   2019-06-12 08:43:59
author: kim-jaehun
categories: [tensorflow]
tags: [tensorflow]
sitemap :
  changefreq : daily
  priority : 1.0
---

##Protocol Buffers란?

`Protocol Buffers`(프로토콜 버퍼)는 구조화 된 데이터를 직렬화하기위한 유연하고 효율적인 자동화 된 메커니즘으로, `XML`보다 작지만 더 빠르고 단순합니다. 또한 다양한 언어(C++, C#, Dart, Go, Java, Python..)를 지원하고 있습니다.

* C++, C#, Dart, Go, Java, Python 별 [Tutorials](https://developers.google.com/protocol-buffers/docs/tutorials)

### Protocol Buffers 사용법

#####0. protobuf 설치
```
pip install protobuf
```

프로토콜 버퍼를 사용하기 위해서는 저장하기 위해 .proto 형태로 정의한다
<br/>
#####1. `addressbook.proto` 생성
```
package tutorial;

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }
  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }
  repeated PhoneNumber phone = 4;
}
message AddressBook {
  repeated Person person = 1;
}

```
각 data_structure 마다 `message` 키워드를 붙이고 각 필드에 대해 이름과 유형을 지정한다.
<br/>
#####2. .proto파일을 컴파일

protoc를 이용해서 파이썬 코드로 컴파일한다.

```
protoc -I=$SRC_DIR --python_out=$DST_DIR $SRC_DIR/addressbook.proto
```
addressbook_pb2.py가 생성 된다.

<br/>
#####3. Write A Message
```
#! /usr/bin/python

import addressbook_pb2
import sys

# This function fills in a Person message based on user input.
def PromptForAddress(person):
  person.id = int(raw_input("Enter person ID number: "))
  person.name = raw_input("Enter name: ")

  email = raw_input("Enter email address (blank for none): ")
  if email != "":
    person.email = email

  while True:
    number = raw_input("Enter a phone number (or leave blank to finish): ")
    if number == "":
      break

    phone_number = person.phones.add()
    phone_number.number = number

    type = raw_input("Is this a mobile, home, or work phone? ")
    if type == "mobile":
      phone_number.type = addressbook_pb2.Person.MOBILE
    elif type == "home":
      phone_number.type = addressbook_pb2.Person.HOME
    elif type == "work":
      phone_number.type = addressbook_pb2.Person.WORK
    else:
      print "Unknown phone type; leaving as default value."

# Main procedure:  Reads the entire address book from a file,
#   adds one person based on user input, then writes it back out to the same
#   file.
if len(sys.argv) != 2:
  print "Usage:", sys.argv[0], "ADDRESS_BOOK_FILE"
  sys.exit(-1)

address_book = addressbook_pb2.AddressBook()

# Read the existing address book.
try:
  f = open(sys.argv[1], "rb")
  address_book.ParseFromString(f.read())
  f.close()
except IOError:
  print sys.argv[1] + ": Could not open file.  Creating a new one."

# Add an address.
PromptForAddress(address_book.people.add())

# Write the new address book back to disk.
f = open(sys.argv[1], "wb")
f.write(address_book.SerializeToString())
f.close()
```

<br/>
#####4. Reading A Message
```
#! /usr/bin/python

import addressbook_pb2
import sys

# Iterates though all people in the AddressBook and prints info about them.
def ListPeople(address_book):
  for person in address_book.people:
    print "Person ID:", person.id
    print "  Name:", person.name
    if person.HasField('email'):
      print "  E-mail address:", person.email

    for phone_number in person.phones:
      if phone_number.type == addressbook_pb2.Person.MOBILE:
        print "  Mobile phone #: ",
      elif phone_number.type == addressbook_pb2.Person.HOME:
        print "  Home phone #: ",
      elif phone_number.type == addressbook_pb2.Person.WORK:
        print "  Work phone #: ",
      print phone_number.number

# Main procedure:  Reads the entire address book from a file and prints all
#   the information inside.
if len(sys.argv) != 2:
  print "Usage:", sys.argv[0], "ADDRESS_BOOK_FILE"
  sys.exit(-1)

address_book = addressbook_pb2.AddressBook()

# Read the existing address book.
f = open(sys.argv[1], "rb")
address_book.ParseFromString(f.read())
f.close()

ListPeople(address_book)
```








<br><br>
#### 참고문헌
* https://developers.google.com/protocol-buffers/docs/overview
* https://bcho.tistory.com/1182
