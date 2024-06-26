---
layout: single
title: "언리얼 Moving Platform"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, C++]
toc: true
toc_sticky: true

Date: 2023-01-24
published: true
---

# Moving Platform

## 움직일 거리 및 속도

내가 움직이고자 하는 플랫폼의 헤더파일에 각 변수를 지정한다.
```cpp
public:
  UPROPERTY(EditAnywhere, Category = "Moving Platform")
  FVector platformVelocity = FVector(100, 0, 0);
  UPROPERTY(EditAnywhere, Category = "Moving Platform")
  float MovedDistance = 100;
```

## 구현
움직임을 구현할 함수도 지정한다.
```cpp
private:
  void MovePlatform(float DeltaTime);
```
이제 cpp 파일로 가서 기능을 구현한다.

내가 원하는 거리만큼만 움직이길 바라기 때문에 시작지점과 현재위치의 거리가

희망하는 거리보다 멀어지면 시작지점과 velocity 값을 반대로 바꾸어준다.
```cpp
void AMovingPlatform::BeginPlay()
{
  Super::BeginPlay();
  StartLocation = GetActorLocation();
}

void AMovingPlatform::Tick(float DeltaTime)
{
  Super::Tick(DeltaTime);
  MovePlatform(DeltaTime);
}

void AMovingPlatform::MovePlatform(float DeltaTime)
{
  FVector CurrentLocation = GetActorLocation();
  CurrentLocation = CurrentLocation + platformVelocity * DeltaTime;
  SetActorLocation(CurrentLocation);

  float Distance = FVector::Dist(StartLocation, CurrentLocation);
  if(Distance > MovedDistance)
  {
    FVector MoveDir = platformVelocity.GetSafeNormal();
    StartLocation = StartLocation + MoveDir * MovedDistance;
    SetActorLocation(StartLocation);
    platformVelocity = -platformVelocity;
  }
}
```

## 결과
![11](https://user-images.githubusercontent.com/87271529/214235938-3bd4f887-b929-4646-97db-20a082bf2a4b.gif)
