---
layout: single
title: "Python tkinter in mac"
categories:
  - PythonStudy
tags:
  - [Python, tkinter, mac]
toc: true
toc_sticky: true

Date: 2023-02-04
published: true
---

# Python tkinter in mac
## tkinter 설치
mac 에 python3 가 설치되어 있더라도 tkinter 가 설치되어 있지 않다.

터미널을 열고 `brew install python-tk` 을 입력하여 tkinter 모듈을 설치한다.

이제 모듈을 사용할 수 있다.

```python
from tkinter import *
window = Tk()
window.mainloop()
```
<img width="450" alt="1" src="https://user-images.githubusercontent.com/87271529/216769806-5ec5857e-9f67-4513-be79-b6fa8b1c15fa.png">

파이썬을 실행하면 다음과 같이 창이 뜬다.

tk 라고 적힌 타이틀을 바꾸고 이미지 하나를 띄워보자.

```python
from tkinter import *
window = Tk()
window.title('Gono Mac')
icon = PhotoImage(file='py.png')
label = Label(window, image=icon)
label.pack()
window.mainloop()
```
<img width="1038" alt="2" src="https://user-images.githubusercontent.com/87271529/216770467-f2bd8161-f246-4130-9074-6cdcf6f8fd23.png">
다음과 같이 타이틀이 바뀌고 이미지의 크기에 맞춰 크기가 조정된다.

이제 버튼을 만들어보자.
```python
#label = Label(window, image=icon)
#label.pack()
button = Button(window, image=icon)
button.pack()
```
라벨을 주석처리하고 버튼을 생성하자.

그럼 이렇게 버튼이 생성된다.

![python_button](https://user-images.githubusercontent.com/87271529/216774253-aee2b788-0664-4858-91be-d3562412c411.gif)

끝!