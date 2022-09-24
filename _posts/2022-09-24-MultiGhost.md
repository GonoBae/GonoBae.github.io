---
layout : single
title : "캐릭터 유령화"
categories:
  - PoliceAndThief
tags:
  - [Unity, PoliceAndThief]

toc: true
toc_sticky: true

Date: 2022-09-24
published: false
---

# 📌 Ghost

어몽어스 게임에서 임포스터에게 죽게되면 유령이 된다.

내 화면에서는 유령으로 , 상대 화면에서는 시체 혹은 안보이게 된다.

이를 Photon (포톤) 으로 구현해보자.

## 📝 Player

우선 Player 는 기본적인 Material 을 가진다.

![Player](https://user-images.githubusercontent.com/87271529/192085120-281b229b-6a82-4366-bdb9-72ef4bf76f27.png)

유령처럼 보이기 위해 Material 의 Alpha 값을 낮추고 Rendering Mode 를 Transparent 로 바꿔 주어야 한다.

코드로 다 바꿔줄 수 있다.

하지만 Texture Atlas 로 인해 죽은 캐릭터를 유령으로 바꾸면 모든 플레이어가 바뀌는 현상이 생기므로

유령에 쓸 Material 을 새로 만들어준다.

## 📝 원리



### 📋 Code

```cs

```

### 💻 Execute


