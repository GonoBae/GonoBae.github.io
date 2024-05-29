---
layout: single
title: "Unreal with CIGI"
categories:
  - UnRealStudy
tags:
  - [Unreal, 언리얼, CIGI]
toc: true
toc_sticky: true

Date: 2024-04-26
published: true
---

# 📌 CIGI

 호스트와 IG 사이의 인터페이스

 호스트 : 시뮬레이션되는 장치에 대한 정보를 제공하는 계산 시스템

 IG (Image Generator) : 렌더링 채널로 구성된 이미지 생성기


## 📋 세팅

호스트 에뮬레이터 : 호스트 대용으로 사용할 수 있음. 엔터티 생성, 수정 및 삭제를 위한 GUI 제공.

Unreal Engine 5 : IG 로 사용할 수 있는 엔진.

[CIGI 사이트를 참고하자.](https://cigi.sourceforge.io/index.php)

해당 사이트에서 CIGI 문서 , CIGI Host Emulator 및 소스 를 확인할 수 있다.

CIGI Host Emulator 는 UDP 를 기반으로 통신하기에 Unreal 에서도 UDP 통신이 필요하다.

Unreal 에서는 Object Deliverer 라는 무료 Plugin 을 사용하여 UDP 통신이 가능하다.


## 📋 이미지

<img src="https://github.com/GonoBae/B_DCS/assets/87271529/abf9d3ba-d44c-4d68-8d53-f4f84e3e2221" align="center" alt="CIGI PDF" width="50%" height="50%">
<figcaption>CIGI PDF</figcaption>

<img src="https://github.com/GonoBae/B_DCS/assets/87271529/8adac6b0-7c46-4ac5-bc0e-b8cd1882d5a6" align="center" width="50%" height="50%">
<figcaption>CIGI Host Emulator</figcaption>

<img src="https://github.com/GonoBae/B_DCS/assets/87271529/bedc05b7-b49e-4ce2-9a3e-423991498f04" align="center" width="50%" height="50%">
<figcaption>CIGI Host Emulator Source</figcaption>

<img src="https://github.com/GonoBae/B_DCS/assets/87271529/cdc4965e-ec6c-46f1-a3e2-36a040d7a70e" align="center" width="50%" height="50%">
<figcaption>Unreal ObjectDeliverer</figcaption>
