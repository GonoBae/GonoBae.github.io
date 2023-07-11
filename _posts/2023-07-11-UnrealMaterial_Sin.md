---
layout: single
title: "언리얼 Material Oscilloscope 파형 만들기"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, C++]
toc: true
toc_sticky: true

Date: 2023-07-11
published: true
---

# 📌 Unreal Material

## 📋 Oscilloscope
<img src="https://github.com/GonoBae/UEStudy/assets/87271529/c214a28e-2b94-4e0a-ad7f-954b544c0184" width="400" height="300">

오실로스코프의 화면과 같이 움직이는 사인파를 만들 일이 있을 수도 있다.

Unreal 에서 Material 을 활용해 만들어보자.

### 격자
격자 문양은 UV 를 가로/세로로 분리해 구현할 수 있다.

Multiply + Frac = 반복 을 이용해 패턴을 만들어준다.

![2](https://github.com/GonoBae/UEStudy/assets/87271529/f8cab759-9a22-49e5-a7c4-ec5770bc44f7)

### Sin파
distance 0.5 는 값이 0.5 인 지점에서 거리가 멀어질수록 0 ~ 1의 값을 가진다.

U 부분은 sin 이 -1 ~ 1 이기 때문에 절반은 0 -> 1 -> 0 , 절반은 0 -> -1 -> 0 이 된다.

여기에 0.2 를 곱해 -0.2 ~ 0.2 값을 가지게 된다.

V 부분은 0 ~ 1 로 증가하기에 이와 합하게 되면 왼쪽 부분은 모두 양수이지만,

오른쪽 부분은 0 -> -0.2 -> 0 으로 곡선이기에 곡선모양으로 검은 부분이 나온다.

V 가 그라데이션 이기에 Distance 0.5 를 하게 되면 중간 부분에서 곡선 모양이 나타난다.

<img src="https://github.com/GonoBae/UEStudy/assets/87271529/0bf5db30-0308-4092-8737-5f29103567e5" width="400" height="300">

각 부분에 색을 곱해주고 Time 을 U 에 더해줌으로써 움직임을 준다.

## 📋 결과
![2](https://github.com/GonoBae/UEStudy/assets/87271529/97c3d01a-9ca7-4558-bfc0-00452ae7b881)