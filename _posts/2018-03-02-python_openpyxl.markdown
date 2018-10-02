---
layout: post
title:  "python_openpyxl"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

### openpyxl란?

Openpyxl is a Python library for reading and writing Excel 2010 xlsx/xlsm/xltx/xltm files.


```
sudo pip3 install openpyxl
```


### Excel 쓰기
```python
from openpyxl import Workbook

# print(ws1["A1"].value)  #셀 A1의 내용 출력
wb = Workbook()
 sheet1 = wb.active
 file_name = 'korea_food.xlsx'
 sheet1.title = 'food_list'

 for i, (food, count) in enumerate(zip(food_list, counts)):
     sheet1.cell(row=i+1, column=1).value = food
     sheet1.cell(row=i+1, column=2).value = count
 wb.save(filename=file_name)
```



### Excel 읽기 & 읽고 쓰기
```python
import openpyxl

# 엑셀파일 열기
wb = openpyxl.load_workbook('score.xlsx')

# 현재 Active Sheet 얻기
ws = wb.active
# ws = wb.get_sheet_by_name("Sheet1")

# 국영수 점수를 읽기
for r in ws.rows:
    row_index = r[0].row   # 행 인덱스
    kor = r[1].value
    eng = r[2].value
    math = r[3].value
    sum = kor + eng + math

    # 합계 쓰기
    ws.cell(row=row_index, column=5).value = sum

    print(kor, eng, math, sum)

# 엑셀 파일 저장
wb.save("score2.xlsx")
wb.close()
```






<br><br>
#### 참고문헌
* https://openpyxl.readthedocs.io/en/stable/
* http://hodubab.tistory.com/92
* http://thrillfighter.tistory.com/457 [C언어 예술가]
