---
layout: single
title: "Unreal Path Trail(Procedural Mesh)2"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, Trajectory, Trail]
toc: true
toc_sticky: true

Date: 2024-02-07
published: true
---

# 📌 Procedural Mesh
```cpp
CreateMeshSection_LinearColor
(
    int32 SectionIndex,
    const TArray< FVector >& Vertices,
    const TArray< int32 >& Triangles,
    const TArray< FVector >& Normals,
    const TArray< FVector2D >& UV0,
    const TArray< FLinearColor >& Vert...,
    const TArray< FProcMeshTangent >& ...,
    bool bCreateCollision
)
```
다음 함수를 이용하여 Mesh를 그려보자.

기본적인 정점과 삼각형만 넣어주자.

## 📋 세팅
다음과 같이 궤적을 생성할 수 있다.

![Trail1](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/83e99217-db34-4688-8063-bbdb6a5411fe)

정점에는 정점을 순서대로 넣어주고 삼각형에는 시계 반대 방향으로 정점을 넣어주면 된다.

정점은 (0, 1, 2, 3, 4, ...)

삼각형은 (0, 1, 2, 3, 2, 1, ...)

이러한 방식으로 작성한다.

```cpp
  void UpdateVertices();

	UPROPERTY()
	UProceduralMeshComponent* proceduralMesh;

	UPROPERTY(BlueprintReadWrite, EditAnywhere, meta = (AllowPrivateAccess = true))
	AActor* Target;
	
	TArray<FVector> vertices;
	TArray<int32> triangles;
	TArray<FVector> normals;
	TArray<FProcMeshTangent> tangents;
	TArray<FVector2D> uv0;
	TArray<FLinearColor> vertexColors;
```

```cpp
void AMyActor::UpdateVertices()
{
  // 큐브의 좌, 우를 구분
	vertices.Add(Target->GetActorLocation() - Target->GetActorRightVector() * 50);
	vertices.Add(Target->GetActorLocation() + Target->GetActorRightVector() * 50);
	
  // 정점 2개일 때
	if(vertices.Num() < 4)
	{
		triangles.Add(0);
		triangles.Add(1);
	}
  // 정점 4개일 때
	else if(vertices.Num() == 4)
	{
		triangles.Add(2);
		for(int i = 2; i >= 0; i--)
		{
			triangles.Add(triangles[i] + 1);
		}
	}
  // 정점 4개 초과일 때
	else
	{
		int to = triangles.Num();
		int from = to - 6;
		for(int i = from; i < to; i++)
		{
			triangles.Add(triangles[i] + 2);
		}
	}

	proceduralMesh->CreateMeshSection_LinearColor(count, vertices, triangles, normals, uv0, vertexColors, tangents, true);
}
```

## 📋 결과
결과는 다음과 같다.

![1](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/3baae9b3-fca9-4d7f-b695-e189d21cfef8)