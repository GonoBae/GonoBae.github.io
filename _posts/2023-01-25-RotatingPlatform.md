---
layout: single
title: "언리얼 Rotating Platform"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, C++]
toc: true
toc_sticky: true

Date: 2023-01-25
published: true
---

# Rotating Platform

## 구현
MovingPlatform 을 구현한 방식으로 만들어보자.

헤더파일에 회전속도와 함수를 선언한다.
```cpp
public:
  UPROPERTY(EditAnywhere, Category = "Rotating Platform")
  FRotator RotateVelocity = FRotator(0, 0, 0);
private:
  void RotatePlatform(float DeltaTime);
```
이제 cpp 파일에서 기능을 구현해주자.
```cpp
void AMovingPlatform::Tick(float DeltaTime)
{
  Super::Tick(DeltaTime);

  MovePlatform(DeltaTime);
  RotatePlatform(DeltaTime);
}
void AMovingPlatform::RotatePlatform(float DeltaTime)
{
  FRotator rot = GetActorRotation();
  rot = rot + RotateVelocity * DeltaTime;
  SetActorRotation(rot);
}
```
이렇게 구현하면 Detail 창에서 원하는 방향(x, y, z)에 속도를 부여하여 회전시킬 수 있다.

하지만 이렇게 구현할 경우 문제가 발생한다.

X(Roll), Z(Yaw) 는 문제가 없으나

Y 축인 Pitch 변수가 -90 ~ 90 까지만 회전하기 때문이다.

Y(Pitch) 에 100 의 속도를 부여하고 실행시키면 다음과 같이 문제가 나타난다.

![11](https://user-images.githubusercontent.com/87271529/214578386-5b58637b-8c72-4809-97f0-513d3ad875c6.gif)

90도 이상 회전하지 못하고 덜덜 떨린다.

## 해결
AddActorLocalRotation() 함수를 통해 회전시키면 해결할 수 있다.

매개변수로 FRotator 나 FQuat 를 담아서 회전시킬 수 있다.

위의 cpp 파일에서 ``RotatePlatform`` 만 수정해주면 된다.

```cpp
AMovingPlatform::RotatePlatform(float DeltaTime)
{
  AddActorLocalRotation(RotateVelocity * DeltaTime);
}
```

Detail 창에서 Y축에 속도를 부여하면 다음과 같이 회전한다.

![22](https://user-images.githubusercontent.com/87271529/214583038-27c45b40-ec19-4205-8d11-f0868d68aebe.gif)

## 참고
FRotator 를 UPROPERTY 를 통해 Detail 창에서 수정하면 XYZ 축을 알맞게 회전시켜 주지만

FRoator(100, 0, 0) 과 같이 코드상으로 넣어주게 되면

(Pitch, Yaw, Roll) 순으로 들어가기에 YZX 순으로 입력이 된다.

따라서 Y축을 기준으로 회전한다.