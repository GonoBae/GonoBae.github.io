---
layout: single
title: "Player Rendering Sort"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼]
toc: true
toc_sticky: true

Date: 2024-01-12
published: true
---

# 📌 Rendering Sort

## 📋 세팅
![First](https://github.com/GonoBae/GonoBae/assets/87271529/a46adbe6-cb55-414d-b96e-2bf63f47b960)

위 사진과 같이 동일선상이 아닌 캐릭터를 앞, 뒤 구별하자.

<img src="https://github.com/GonoBae/GonoBae/assets/87271529/b583b7c9-ff3a-424d-a252-7bde8193c0d5" width="300" height="300"/>
<img src="https://github.com/GonoBae/GonoBae/assets/87271529/ab392bee-4f1e-415d-9b38-70ec7fa4bc2e" width="300" height="300"/>

위 사진과 같이 나타내는 기능이다.

언리얼에서 렌더링 우선순위를 부여해 렌더 순서를 지정할 수 있다.

이를 이용하자.

Sprite 설정창으로 들어가면 Pivot Mode 이 있다.

기본적으로 Center Center 로 설정되어 있다.

캐릭터 크기가 다양하며 이를 발바닥 기준으로 앞/뒤를 비교하기 위해 Bottom Center 로 바꿔준다.

![Pivot](https://github.com/GonoBae/GonoBae/assets/87271529/91cd441b-22d4-45df-ab13-8ebbce7c79f3)

이를 설정해주어야 언리얼의 Rendering Sort 를 사용할 수 있다. (필수!!)

그리고 Character 블루프린트를 만들면 Character Movement의 Capsule Component 의 Location이

(0,0,0)이라 이에 Sprite 를 맞추면 Sprite Location 이 (0,0,-5) 와 같이 Sprite를 내려주어야 한다.

이렇게 Sprite를 낮추게 되면 캐릭터의 위치 비교에 문제가 생긴다.

(테스트했을 때 발바닥이 아닌 Sprite가 내려간만큼 올라간 Pivot(몸통) 에서 비교가 되었다.)

그렇기에 Sprite 의 위치를 고정한다.

Capsule 의 위치와 Sprite 의 위치가 맞지 않기 때문에 충돌이 필요할 경우

Capsule Component 를 NoCollision 으로 설정하고 별도의 BoxCollision 을 사용한다.

![Material](https://github.com/GonoBae/GonoBae/assets/87271529/7c4a1d87-6ca4-493d-aefe-364edd64b2ab)


### 배치
![Map](https://github.com/GonoBae/GonoBae/assets/87271529/ac498527-81f0-44fc-95fd-b186ceb14c87)

다음과 같이 맵이 배치되었을 경우에서 테스트 해보자.

위로 올라갈수록 뒤, 아래로 내려올수록 앞이어야 하는 탑뷰이다.

캐릭터가 아래로 내려올수록 렌더링 우선순위가 높아져야 하므로 (400 - 캐릭터 Z 값)을 해준다.

![Priority](https://github.com/GonoBae/GonoBae/assets/87271529/4a25f321-43c0-43f3-b05b-698219bda5bb)

움직일 때마다 Z 값이 바뀌는 캐릭터는 입력 혹은 Tick 에서 계속해서 갱신해준다.

![PlayerBP](https://github.com/GonoBae/GonoBae/assets/87271529/510a88bd-d5a4-4ddd-b3b2-27ec54cb54bf)

NPC 처럼 제자리에 가만히 있는 Actor 는 BeginPlay 혹은 Initialize 함수에서 세팅한다.

![NPCBP](https://github.com/GonoBae/GonoBae/assets/87271529/6e9b1b2a-21b6-493c-be70-8581fdef50d5)

## 📋 결과
![Test](https://github.com/GonoBae/GonoBae/assets/87271529/5452c5eb-836f-4241-a866-c016553b79c7)