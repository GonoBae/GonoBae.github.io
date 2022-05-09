---
layout: single
title: "[코딩테스트] Level2 : 조건문"
categories: CordingTest
tag: CordingTest
---

이번 포스트는 단계별 분류의 제 2번째 이다. <br>
총 7문제로 구성되어 있다. <br>

### [C#] [C++]
int a, b, c; <br>
비교 1. a == b == c <br>
비교 2. a == b && a == c && b == c <br>
위 비교 1과 2의 다른 점에 대해 알아보게 되었다. <br>
if문에서 a == b 가 되었을 때 true를 반환하게 된다. <br>
그러면 true == c 가 되는데 c는 int형으로 이 if문은 false가 된다. <br>
단순 수학적인 계산과는 다르다는 점 기억하자. <br>


![SecondLevel](../../images/2022-05-03-CordingTestLevel2/SecondLevel.PNG)