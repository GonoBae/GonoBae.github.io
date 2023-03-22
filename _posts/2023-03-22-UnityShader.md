---
layout: single
title: "Unity Shader"
categories:
  - UnityStudy
tags:
  - [Unity, 유니티, C#]
toc: true
toc_sticky: true

Date: 2023-03-22
published: true
---

# 📌 유니티 쉐이더 (Vertex & Fragment)

## 📋 Shader 기본 구조

```cs
Shader "MyCustomShader"
{
  Properties
  {
    // 사용자 인터페이스
    // Inspector 창에서 입력값을 조절할 수 있다.
  }
  // 고성능 디바이스에서는 상위의 셰이더가 우선 적용
  SubShader
  {
    Pass
    {
      // GPU 에게 넘겨줄 작업을 기술
      // 두개 이상이면 멀티 패스라고 하며, 하나의 물체에 Pass 수만큼 셰이더 작업을 수행
    }
  }
  SubShader
  {
    Pass
    {
      // ... //
    }
  }
  Fallback "Diffuse" // 가장 낮은 품질의 셰이더
}
```
유니티는 렌더링 할 때 가장 위의 SubShader(최상품질)부터 확인하여 적합한 스펙을 고른다.

만약 적합한 스펙이 없다면 Fallback 에 적혀있는 셰이더를 사용한다.

## 📋 Vertex/Fragment Shader 구조

```cs
Shader "MyCustomShader"
{
  Properties { }
  SubShader
  {
    Pass 
    {
      CGPROGRAM
        // CG Code
      ENDCG
    }
  }
}
```
기본 구조와 다른 점은 CGPROGRAM-ENDCG 가 추가되었다는 것이다.

이 부분이 vertex/fragment program 에 대한 것이다.

## 📋 
```cs
CGPROGRAM
  #pragma vertex vert
  #pragma fragment freg
  // CG Code
ENDCG
```
#pragma 는 컴파일 지시자를 뜻한다.

vert 는 버텍스, frag 는 프래그먼트 프로그램(함수)이다.

지시자를 통해 이 함수들을 CG 코드로 작성할 것이라 지시한다.

vert, frag 등 함수의 이름은 내 맘대로 바꿀 수 있다.

```cs
CGPROGRAM
  #pragma vertex vert
  #pragma fragment freg
  
  // CG Code
  struct appdata { }
  struct v2f { }

  v2f vert(appdata v)
  {
    // ... //
  }

  fixed4 frag(v2f i) : SV_Target
  {
    // ... //
  }
ENDCG
```
다음과 같이 지시자에 선언한 함수명으로 함수를 작성할 수 있다.

기본적으로 Vertex Shader 가 그려질 위치를 결정하고 Fragment Shader 가 칠할 색을 결정한다.

지시자로 선언한 vert / frag 함수는 우리가 호출하는 것이 아니라 

자동으로 호출되어 작동하기에 원하는 방식으로 구현하면 된다.

