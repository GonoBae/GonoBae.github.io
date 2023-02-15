---
layout: single
title: "언리얼 Rotating Raycast"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, C++]
toc: true
toc_sticky: true

Date: 2023-02-16
published: true
---

# 📌 회전하는 레이캐스트

## 📋 Raycast
해당 기능을 재사용 하기 위해 컴포넌트로 만든다.
```cpp
class BLOGPROJECT_API URaycast : public USceneComponent
{
public:
  UPROPERTY(EditAnywhere)
  float Distance;
  FVector StartPos;
  FVector Forward;
  FCollisionQueryParams Params;
  FCollisionObjectQueryParams ObjectParams;
  FHitResult HitResult;
};
```
`FCollisionQueryParams` 와 `FCollisionObjectQueryParams` 는 충돌체 간의 설정을 하는 구조체이다.

```cpp
void URaycast::BeginPlay()
{
  Super::BeginPlay();

  StartPos = GetComponentLocation();
  // 자신은 충돌에서 제외
  Params.AddIgnoredActor(GetOwner());
  // PhysicsBody 를 충돌하도록 추가
  ObjectParams.AddObjectTypesToQuery(ECollisionChannel::ECC_PhysicsBody);
}

void URaycast::TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction)
{
  Super::TickComponent(DeltaTime, TickType, ThisTickFunction);

  Forward = GetForwardVector() * Distance;

  if(GetWorld()->LineTraceSingleByObjectType(HitResult, StartPos, StartPos + Forward, ObjectParams, Params))
  {
    DrawDebugLine(GetWorld(), StartPos, StartPos + Forward, FColor::Green, false, 0.01f, 0, 5.0f);
  }
  else
  {
    DrawDebugLine(GetWorld(), StartPos, StartPos + Forward, FColor::Red, false, 0.01f, 0, 5.0f);
  }
}
```
![3](https://user-images.githubusercontent.com/87271529/219084128-0c8616ec-ce13-4430-8db0-204019898016.PNG)

다음과 같이 컴포넌트를 추가해주고 Distance 를 설정한다.

![2](https://user-images.githubusercontent.com/87271529/219083044-b36f08a1-f0d5-4668-ac77-e934429d1d20.PNG)

![1](https://user-images.githubusercontent.com/87271529/219082915-74810109-435a-4c32-baf9-1adade849a59.PNG)

Raycast 가 ObjectType 을 PhysicsBody 로 설정한 큐브와 충돌하면

위의 사진과 같이 동작한다.

이제 회전을 주어 레이더와 같이 작동하도록 만들어보자.

## 📋 Rotate
```cpp
public:	
  UPROPERTY(EditAnywhere)
  FRotator RotateVelocity = FRotator(0, 0, 0);
```
헤더에 다음과 같이 추가하고

```cpp
void URaycast::TickComponent(float DeltaTime, ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction)
{
  ...

  Rot = GetOwner()->GetActorRotation();
  Rot = Rot + RotateVelocity * DeltaTime;
  GetOwner()->SetActorRotation(Rot);

  ...
}
```
cpp 파일에 다음과 같이 추가하고 작동시켜보자.

## 💻 실행
![111](https://user-images.githubusercontent.com/87271529/219090256-9a84b745-7acc-4579-a492-ac4555acde25.gif)