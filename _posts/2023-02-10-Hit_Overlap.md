---
layout: single
title: "언리얼 Hit"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, C++]
toc: true
toc_sticky: true

Date: 2023-02-10
published: true
---

# 언리얼의 충돌 필터링

언리얼에서 충돌을 다뤄보면 다음 화면을 마주하게 된다.

![5](https://user-images.githubusercontent.com/87271529/217900286-8128a03e-3f9b-499d-80e3-984b880d5de3.jpg)

Ignore 는 무시같은데 Overlap 과 Block 은 어떤 차이를 가지고 있는지

정확하게 몰랐기에 찾아보게 되었다.

![1](https://user-images.githubusercontent.com/87271529/217899938-d5e59d62-8f6d-434b-a33a-65b291b7f482.jpg)

위의 그림을 보면 쉽게 이해할 수 있다.

총알은 잔디를 지나 유리에 부딪치게 되지만

우리가 바라보는 화면은 잔디에 막혀 유리를 볼 수 없게 된다.

하지만 유리를 통과해 벽을 볼 수 있다.

이처럼 직접적인 충돌이 발생하는 것은 Block, 막히지는 않고 겹칠 수 있게 되는 것은 Overlap 으로 지정한다.

![3](https://user-images.githubusercontent.com/87271529/217901783-75ce220e-a9a7-41bd-82a4-50491b5ac0bd.jpg)

만약 플레이어가 앞으로 나아간다면 어떻게 될까?

Player 는 WorldStatic 을 Block 처리했다.

하지만 WorldStatic 인 Shrub 과 Wall 은 다르다.

Shrub 은 Pawn 을 Overlap , Wall 은 Pawn 을 Block 한다.

![2](https://user-images.githubusercontent.com/87271529/217902211-eaf821be-e827-45a4-8945-30c60c5b75e7.jpg)

위의 표를 보면 서로 Block 인 경우가 아니라면 Block 처리가 되지 않는다는 것을 알 수 있다.

따라서 Player 는 Shrub 을 지나쳐 Wall 에 충돌하게 된다.

이 때 다음과 같은 이벤트가 발생한다.

![4](https://user-images.githubusercontent.com/87271529/217902664-3f78fd82-9bb6-4776-9b24-c1885c83c1c0.jpg)

각각의 상황에 맞는 이벤트를 사용해 충돌을 확인할 수 있다.

마치 Unity 의 Trigger 와 Collision 의 차이를 보는 것 같다.

- 참고 [원글링크](https://www.unrealengine.com/en-US/blog/collision-filtering)

