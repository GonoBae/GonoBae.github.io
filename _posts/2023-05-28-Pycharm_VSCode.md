---
layout: single
title: "Pycharm 프로젝트를 VSCode 로 열기"
categories:
  - PythonStudy
tags:
  - [Python, Pycharm, VSCode]
toc: true
toc_sticky: true

Date: 2023-05-28
published: true
---

# Pycharm 프로젝트 VSCode 로 열기
## Pycharm -> VS Code

Pycharm 은 매우 편리하다.

모듈설치도 `import` 코드에 마우스를 갖다대면 바로 설치가 가능하다.

그런데 해당 프로젝트를 VS Code 로 열어보니 module 을 찾을 수 없다는 경고가 떴다.

이를 해결하고 기록해보자.

### 문제

```python
# pandas 모듈 프로젝트
import pandas as pd

series = pd.Series({'a': 1, 'b': 2})

print(series.values)
```
다음과 같이 pandas 모듈을 사용하였다고 가정해보자.

Pycharm 에서는 잘 동작하던 모듈이 VS Code 에서는 작동하지 않는다.

모듈을 찾을 수 없다는 경고가 뜬다.

<img width="278" alt="S1" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/f4704316-6378-460e-8277-605e218fa1f9">

이렇게 찾을 수 없는 이유는 Pycharm 같은 경우 프로젝트를 생성할 때

venv 라는 폴더가 생기는데 모듈을 venv/bin/lib 위치에 설치하기 때문이다.

### 분석

```python
# sys 모듈을 통한 경로 확인하기
import sys

print(sys.path)
```

이렇게 출력한 경로는 프로젝트가 참조할 경로들을 보여준다.

경로를 쭉 살펴보면 Pycharm 이 module 을 설치한 venv 를 포함한 경로가 보이지 않는다.

우리가 이 경로를 직접 포함시켜 주어야 한다.

### 해결
```python
# sys 모듈을 통한 경로 확인하기
import sys
sys.path.append('/Users/baegono/Desktop/Python/
    pythonProject/venv/lib/python3.9/site-packages')
print(sys.path)
```

이렇게 venv 경로 내의 사용하고 있는 파이썬 버전의 `site-packages`를 추가해준다.

이제 정상적으로 작동한다.

끝!