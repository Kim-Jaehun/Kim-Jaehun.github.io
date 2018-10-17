---
layout: post
title:  "python xlrd,xlwd(엑셀 읽기, 쓰기)"
date:   2018-03-02 08:43:59
author: kim-jaehun
categories: [Android]
tags: Fragment
---

###xlrd
This package is for reading data and formatting information from older Excel files (ie: .xls)

###xlwt
This package is for writing data and formatting information to older Excel files (ie: .xls)

* Linux에서도 사용 가능
* Excel 97 / 2000 / XP / 2003 포맷(.xls)만 지원

```
sudo pip3 install xlrd
sudo pip3 install xlwd
```


### Excel 쓰기
```python
import xlwt

workbook = xlwt.Workbook(encoding='utf-8') #utf-8 인코딩 방식의 workbook 생성
workbook.default_style.font.height = 20*11   #(11pt) 기본폰트설정 다양한건 찾아보길

worksheet = workbook.add_sheet(sheetname) #시트 생성

font_style = xlwt.easyxf('font: height 280, bold 1;') #폰트 스타일 생성
worksheet.row(index).set_style(font_size) #줄에 폰트 스타일 설정

worksheet.col(index).width = 256*(len(key)+1) #(key byte) 칸 너비 설정

worksheet.write(row_num,col_num,value) #셀 한칸에 값 생성
worksheet.write_merge(top_row_num,bottom_row_num,left_col_num,right_col_num,value) #합쳐진 셀에 값 생성

workbook.save(filename) #엑셀 파일 저장 및 생성
```



### Excel 읽기

```python
import xlrd

workbook = xlrd.open_workbook(filename)

print workbook.sheet_name() #시트목록을 출력(list형식)

worksheet_name = workbook.sheet_by_name(sheetname) #시트이름으로 시트 가져오기
worksheet_index = workbook.sheet_by_index(index) #시트번호(인덱스)로 시트 가져오기

num_rows = worksheet_name.nrows #줄 수 가져오기
num_cols = worksheet_name.ncols #칸 수 가져오기

row_val = worksheet_name.row_values(row_index) #줄 값 가져오기(list형식)
cell_val = worksheet_name.cell_value(row_index,cell_index) #셀 값 가져오기
```


#### Excel 시간 형식 읽기

```python
date_values = datetime(*xlrd.xldate_as_tuple(row_data[2],workbook.datemode))
str_datatime = date_value.strftime("%Y-%m-%d")
```




<br><br>
#### 참고문헌
* http://lorieliot.tistory.com/1
* https://datascienceschool.net/view-notebook/5524dd924b9e4a3399a33b5e70269458/
