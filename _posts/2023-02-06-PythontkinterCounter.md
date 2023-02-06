---
layout: single
title: "Python tkinter 로 카운터 만들기"
categories:
  - PythonStudy
tags:
  - [Python, tkinter, mac]
toc: true
toc_sticky: true

Date: 2023-02-06
published: true
---

# Python tkinter Counter
## Button
버튼에 기능을 추가해보자.
```python
from tkinter import *
window = Tk()
button = Button(window, command={함수이름})
```
을 통해 버튼을 생성하고 해당 버튼에 함수의 기능을 넣어줄 수 있다.

```python
from tkinter import *

count = 0

def click():
  global count
  count += 1
  print(count)

window = Tk()
button = Button(window, text='Click Me!', command=click)
button.pack()
window.mainloop()
```

![clickbutton](https://user-images.githubusercontent.com/87271529/216950541-37a6502f-ef6b-4c0d-bffc-4a664187d9b3.gif)

이렇게 버튼을 통해 함수를 실행시킬 수 있다.

터머널을 통해 카운트되는 숫자를 확인하는 것이 아니라 

우리가 실행시킨 창을 통해 결과값을 확인하면 좋을 것 같다.

라벨 텍스트에 count 변수를 띄워보자.

```python
from tkinter import *

count = 0

def click():
  global count
  count += 1
  var.set(str(count))
  print(count)
    
window = Tk()
button = Button(window, text='Click Me!', command=click)
button.pack()
var = StringVar()
var.set('0')
label = Label(window, textvariable=var)
label.pack()
window.mainloop()
```

![clickcounternum](https://user-images.githubusercontent.com/87271529/216967385-ab2302e9-3db1-4e2e-a8c6-bfb07a1500a4.gif)

tkinter 에서 string 변수값을 나타내는 StringVar 변수형을 통해 count 값을 받아온다.

받아온 값을 라벨에서 사용하면 된다.