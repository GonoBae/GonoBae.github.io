---
layout: single
title: "Unreal Path Trail(Procedural Mesh)"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, Trajectory, Trail]
toc: true
toc_sticky: true

Date: 2024-02-06
published: true
---

# 📌 Trail
DCS 같은 비행 시뮬레이터를 하면 디브리핑을 접할 기회가 생긴다.

이 때 다음과 같이 비행 기체의 궤적을 표현한 것을 볼 수 있다.

![다운로드](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/e6bee1bb-cb40-440d-8ca6-347be4749c95)

이를 언리얼에서 어떻게 구현할 수 있을지 고민하다 부드럽게 생성되는 효과를 위해 Procedural Mesh를 이용하였다.

실제 Unreal 상에서 구현한 모습은 아래 사진과 같다.

![화면 캡처 2024-02-06 222956](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/bc1b7e67-e427-49f7-a67b-2ea16291550d)

구현 원리를 되뇌일겸 구현해보자.

## 📋 세팅
우선 procedural mesh 를 이용하기 위해 mesh를 생성하는 원리를 파악해보자.

mesh 를 그릴 때 기본적으로 필요한 것은 `Vertex` + `Triangle` 이다.

![화면 캡처 2024-02-06 224618](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/d8dbaa4b-2382-4f2a-ac85-bad6b46c80b2)

다음과 같은 삼각형을 그린다고 가정해보자.

![화면 캡처 2024-02-06 224618](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/37b93aea-dc39-4525-9330-f4e1a34fb4c0)

해당 삼각형은 3개의 정점을 가지고 이를 통해 하나의 삼각형을 그리게 된다.

해당 정점을 넣는 방향에 따라 mesh가 바라볼 방향(Normal)이 결정된다.

삼각형 정점의 인덱스를 시계 반대 방향으로 정렬하는 것이 컴퓨터 그래픽의 일반적인 관례라고 한다.

만약 시계 방향으로 정점을 추가할 경우 삼각형이 반대 방향을 바라보기에 material을 two side로 설정하지 않으면 렌더되지 않는다.