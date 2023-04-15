---
layout: single
title: "Python 시간 경과된 파일 삭제하기"
categories:
  - PythonStudy
tags:
  - [Python, os, datetime, mac]
toc: true
toc_sticky: true

Date: 2023-04-15
published: true
---

# Python 으로 경과된 시간에 따라 파일 삭제하기
## os/datetime

```python
import os
import datetime

path = '/Users/baegono/Desktop/Python/PythonBlog/TextFile'

today = datetime.datetime.now()

for file in os.listdir(path):
    file_path = os.path.join(path, file)
    if os.path.isfile(file_path):
        # 파일 생성 시간 출력
        file_creation_date = datetime.datetime.fromtimestamp(os.path.getctime(file_path))
        # 파일 경과된 날짜 출력
        days_ago_created = (today - file_creation_date).days
        if days_ago_created > 10:
            print(f"{file_path} : 파일이 삭제됩니다.")
            os.remove(file_path)
```
`file_creation_date` 을 보면 `os.path.getctime(file_path)` 가 보인다.

이는 파일의 생성 시간을 Unixtime 으로 반환하기에 print() 를 통해 찍어보면 알아볼 수 없다.

그래서 datetime 모듈의 `fromtimestamp()` 를 통해 날짜로 변환할 수 있다.

참고로 Unixtime 은 1970년 1월 1일부터 몇 초가 흘렀는지를 나타내는 수치라고 한다.

`days_ago_created` 을 보면 `(today - file_creation_date)` 가 있는데

이는 현재 시간에서 파일의 생성 시간이 얼마나 흘렀는지 초 단위로 출력하기 때문에 

`.days` 를 통해 하루 단위로 출력하게 된다.

따라서 위의 코드는 10일이 지난 파일을 삭제하는 코드이다.