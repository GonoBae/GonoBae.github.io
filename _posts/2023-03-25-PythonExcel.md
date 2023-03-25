---
layout: single
title: "Python Excel 읽기"
categories:
  - PythonStudy
tags:
  - [Python, pandas, mac]
toc: true
toc_sticky: true

Date: 2023-03-25
published: true
---

# Python pandas
## pandas module
pandas 를 통해 엑셀을 읽어와보자.

<img width="252" alt="1" src="https://user-images.githubusercontent.com/87271529/227717185-36bdaead-fee7-4983-9ebf-d4caa48f89d5.png">

읽고자 하는 폴더의 엑셀파일은 다음 두 파일이다.

일단 파일 하나하나를 열어서 확인해보자.

우선 엑셀파일에 어떤 시트가 존재하는지 확인해보자.
```python
import pandas

excel_folder = '/Users/baegono/Desktop/Python/PythonBlog/ExcelFile'

print(pandas.read_excel(excel_folder + '/Programmer.xlsx', sheet_name=None).keys())
```
read_excel 함수는 내부에서 `openpyxl` 모듈을 사용하기에 해당 모듈을 다운받아야 한다.

<img width="256" alt="1" src="https://user-images.githubusercontent.com/87271529/227596052-ec22d521-c1d4-4cb8-acef-21f50eecc5db.png">

이제 각각의 시트를 열어보자.
```python
import pandas

excel_folder = '/Users/baegono/Desktop/Python/PythonBlog/ExcelFile'

print(pandas.read_excel(excel_folder + '/Programmer.xlsx', sheet_name='Editor'))
```

경로는 말 그대로 경로이고 sheet_name 은 `None = 전체 시트 / 0~N = 시트 인덱스 / 시트이름` 으로 불러올 수 있다.

<img width="215" alt="1" src="https://user-images.githubusercontent.com/87271529/227584140-ca4ecb30-064b-4139-ba1b-50cedbe7298e.png">

다음과 같이 테이블 형식으로 읽어올 수 있다.

위의 코드처럼 파일이름을 지정해서 읽어오는 것은 굉장히 비효율적이다.

`os module` 을 통해 해당 경로에 존재하는 파일들을 가져와서 한번에 읽어보자.

```python
import os

excel_folder = '/Users/baegono/Desktop/Python/PythonBlog/ExcelFile'

walk = os.walk(excel_folder)
for w in walk:
    print(w)
```
우선 os.walk() 함수에 대해 알아보자.

위의 코드를 실행시키면 다음과 같은 결과를 출력한다.

<img width="912" alt="1" src="https://user-images.githubusercontent.com/87271529/227711068-f100973a-3c58-462a-b25d-2be93047b4d8.png">

이는 os.walk() 가 `경로/경로 아래의 폴더/경로 아래의 파일` 을 tuple 로 반환하기 때문이다.

이를 활용하여 엑셀파일만을 뽑아보자.

```python
for path, dirs, files in os.walk(excel_folder):
  for file in files:
    if file.endswith('.xlsx'):
      print(file)
```

<img width="148" alt="1" src="https://user-images.githubusercontent.com/87271529/227717404-01ab469c-874f-4ac5-a03c-7c2cbb622578.png">

폴더에 저장된 두 개의 엑셀 파일이름이 출력된다.

이제 파일을 읽어보자.
```python
for path, dirs, files in os.walk(excel_folder):
  for file in files:
    if file.endswith('.xlsx'):
      excel_file = pandas.read_excel(
        path + '/' + file, 
        sheet_name=None)
      print(excel_file)
```
이전에 사용했던 `read_excel()`함수를 사용하면 다음과 같이 출력된다.
<img width="502" alt="1" src="https://user-images.githubusercontent.com/87271529/227719459-a24de038-b080-4e97-96f5-d4398b658fb5.png">

테이블 형식으로 이쁘게 출력되지 않는다.

방법을 조금 바꿔서 출력해보자.

```python
for path, dirs, files in os.walk(excel_folder):
  for file in files:
    if file.endswith('.xlsx'):
      excel_file = pandas.ExcelFile(path + '/' + file)
      for sheet in excel_file.sheet_names:
        dataframe = excel_file.parse(sheet)
        print(dataframe)
```
<img width="235" alt="1" src="https://user-images.githubusercontent.com/87271529/227719718-5d3fcd70-91e1-4e5e-8cd2-e97925d0167f.png">

이제 보기 편하게 출력이 되었다.

이러한 방법으로 원하는 경로의 엑셀파일을 한번에 읽어볼 수 있다.

각 함수는 다양한 옵션을 가지고 있기에 원하는 부분만을 출력할 수도 있다.