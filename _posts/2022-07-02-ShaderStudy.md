---
layout : single
title : "Amplify Shader 시작하기"
categories:
  - AmplifyShader
tags:
  - [Unity, Amplify Shader, Shader]

toc: true
toc_sticky: true

Date: 2022-07-02
published: true
---

# 📌 Amplify Shader

## ✏️ Shader 란?
![2](https://user-images.githubusercontent.com/87271529/176935151-e71c1778-cd06-4ce4-abfe-9a0836a9bb0a.gif)

게임을 하다보면 물, 유리, 구름, 이펙트 등 단순히 표현하기 힘든 것들이 존재한다.

Unity 에서 Material 을 건든다고 만들 수 있는 것들이 아닌

좀 더 현실감있게 구현하기 위해서는 Shader 가 꼭 필요하다.

Unity 에서는 Shader Graph 라는 Pakage 를 제공한다.

![2](https://user-images.githubusercontent.com/87271529/176932696-e9425420-d7da-4ae2-adca-495dd2fa7c40.png)

설명을 보면 HDRP 혹은 URP 만 제공을 한다고 적혀있다.

Buit-In 에서는 쉐이더 그래프를 다룰 수 없다.

하지만 Amplify Shader Asset 을 구매하면 가능하다. (흑우처럼 사버림)

그래프가 아닌 스크립트로 작업할 수 있지만 그래프가 더 재밌어보인다. :)

## 📝 메모
World Position 의 중심 = 세계 중심 (원점)

Vertex Position 의 중심 = 해당 메시의 피벗 위치

**차이를 느낄 수 있게 (1, 0, 0) 에 Sphere 를 배치한다.**

## 📝 World Position

### 📋 Shader Graph
![45](https://user-images.githubusercontent.com/87271529/176940863-8f4b8b87-5968-4fa2-8acd-9edad3e4ebbe.png)

### 💻 Execute
![3](https://user-images.githubusercontent.com/87271529/176940896-29d8e5c9-7ce0-4276-9352-85e86c517fe5.gif)

(1, 0, 0) 에 위치함에도 (0, 0, 0) 을 기준으로 행동을 반복한다.

## 📝 Vertex Position

### 📋 Shader Graph
![ScreenshotASE](https://user-images.githubusercontent.com/87271529/176941035-b90bfb2f-ee8e-494b-982c-dc3eca1590b8.png)

### 💻 Execute
![3](https://user-images.githubusercontent.com/87271529/176941048-6d590bb4-481a-4d1d-bd7d-c4622044f8c8.gif)

자기 자신의 기준 (1, 0, 0) 을 기준으로 행동을 반복한다.

## 📝 Shader Graph 설명
두 그래프 모두 Time 노드에서 시간마다 Sin 그래프 (-1.0 ~ 1.0) 의 값을 반복적으로 반환한다.

이와 각 포지션을 Multiply (곱셈) 하여 행동한다.

-1.0 ~ 1.0 을 반환하는거면 Scale 이 -1 로 뒤집혀서 반대로 그려져야 하는거 아닌가?

라고 생각할 수 있는데 그게 맞다.

![5](https://user-images.githubusercontent.com/87271529/176942443-6834eefb-3779-46b9-a0f7-daa8c6f25271.png)

하지만 현재 연결된 곳은 Local Vertex Offset 으로 말 그대로 Offset 이다.

현재 메쉬 기준으로 -1 만큼 작아졌다 1 만큼 커지게 되므로

현재 메쉬가 1 이라면 0 에서 2 로 사라졌다 커지기를 반복하는 것이 맞다.

Offset 을 생각하고 노드를 구성하게 되면 다음과 같다.

## 📝 Shader Graph (Offset 고려/World Position)
Vertex Position 은 자기 위치에서 Scale -1 ~ 1 을 반복하기에 촬영에도 티가 안난다.

그래서 World Position 으로 해보았다.

### 📋 Shader Graph
![7](https://user-images.githubusercontent.com/87271529/176943740-2348c8ca-7aac-49dd-a9b1-38fd9836ec1e.png)

### 💻 Execute
![55](https://user-images.githubusercontent.com/87271529/176943765-ff795be4-4ee1-40f9-94ed-646d533fd72b.gif)

## 📝 느낀점
노드에 대한 설명만 보니 전혀 이해가 되지 않았다. ㅠㅠ

여러가지 효과들을 만들어보며 공부하는 것이 좋을 것 같다!
