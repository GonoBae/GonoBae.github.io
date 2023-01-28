---
layout: single
title: "Python Game"
categories:
  - PythonStudy
tags:
  - [Python]
toc: true
toc_sticky: true

Date: 2023-01-29
published: true
---

# Python (rock, scissors, paper) Game

## 구현

코딩을 처음 시작할 때 게임을 만들며 재밌게 공부했었다.

파이썬을 시작했으니 이를 이용해 간단한 게임을 만들어보자.

비슷한 프로그램을 다른 언어로 만들 수 있다는 것이 굉장히 매력적이라고 느껴진다.

```python
import random
from enum import IntEnum
class Action(IntEnum):
    rock = 0
    scissors = 1
    paper = 2
choices = {
        "rock":Action.rock,
        "scissors":Action.scissors,
        "paper":Action.paper
        }
def get_choice(input_data):    
    return choices[input_data]
while True:
    computer = random.randint(0, 2)
    player = ""
    player_choice = -1
    while player not in choices.keys():
        player = input("Enter (rock, scissors, paper) : ").lower()
    player_choice = get_choice(player)
    number = player_choice - computer
    print(player + " VS " + Action(computer).name)
    if number == -1 or number == 2:
        print("You Win!!")
    elif number == 0:
        print("Tie!!")
    elif number == 1 or number -2:
        print("You Lose!!")
    if not input("Play again? (y/n) ").lower() == 'y':
        break
print("Thanks for playing")
```
다양한 방법이 있겠지만 조금이라도 다르게 만들어보고 싶어서 

enum 과 Dictionary 를 사용해보았다.

## 실행
<img width="435" alt="스크린샷 2023-01-29 오전 1 49 37" src="https://user-images.githubusercontent.com/87271529/215278705-f3d383c4-4d22-46b9-8f82-16ed6e4d8c59.png">