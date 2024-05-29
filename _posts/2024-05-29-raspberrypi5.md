---
layout: single
title: "Raspberry pi5"
categories:
  - Raspberry pi5
tags:
  - [Raspberrypi5, pi5]
toc: true
toc_sticky: true

Date: 2024-05-29
published: true
---

# 📌 Home Server
사용하고 싶은 기능은 다음과 같다.

1. 집에서 웹 서버를 돌리고 싶음.

2. 외부에서 코딩할 수 있도록 환경을 구축하고 싶음

## 📋 Raspberry pi5
원래 `라즈베리 파이4` 를 선택하려고 했다.

`라즈베리 파이5` 가 나온지 얼마 되지 않아 정보가 많지 않았고,

참고할 자료가 많은 `라즈베리 파이4` 가 더 나을 것 같다는 생각을 했다.

하지만 `라즈베리 파이5` 가 성능적으로 훨씬 우수했고 SSD 를 추가적으로 장착할 수 있었다.

가격이 살짝 사악하다는 말이 꽤 있었고 이 때문에 `n100`과 같은 `mini PC` 와 비교되기도 했다.

하지만 개인적으로 `라즈베리 파이5` 가 가성비가 가장 괜찮다는 생각을 했다.

![bench](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/046ed20f-58b9-42f3-b99a-aa1a5e3cfaa5)

## 📋 하드웨어
### 부품 목록
- 라즈베리 파이5
- 알루미늄 방열 케이스
- 엑티브 쿨러
- SSD 500GB (Ubuntu 24.04)
- SD 카드 (Rasbian)
- Micro HDMI
- LAN 선

### SSD
![IMG_0040](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/67bc4e7b-48ee-4798-bd10-8b35f92b3400)

SSD HAT 모듈은 여러가지가 있었지만 가장 무난한 `X1001`을 선택했다.

실제 모듈을 장착 후 테스트 해보았다.

![ssd boot1](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/8bfb84ef-b0b1-49a3-9cc4-754baee5fe03)

![ssd boot2](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/f82f8935-e31c-49e3-a9e7-2ca02b2d3a46)

pcie 3.0 설정을 해주어야 속도를 올릴 수 있다.

### 케이스
![IMG_0038](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/ec5cf9c5-955d-43a3-b4b8-ed3890ad0dbc)
라즈베리 파이5 와 호환되는 케이스 중 가장 예쁘다고 생각했다.

(쿨링 기능까지 갖춘 케이스가 없었다.)

하지만 해당 케이스는 `SSD HAT`을 지원하지 않는다.

혹시나 싶어 문의를 해보았으나 `SSD HAT`을 사용할 수 없다는 답변을 받았다.

![IMG_0032](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/4c488bde-9c3e-41f4-84da-08ac17e8f360)

어찌저찌 사용은 할 수 있으나 위 사진처럼 쿨러를 부착할 수 없다.

(공식 엑티브 쿨러를 사용해야 한다.)

![IMG_0035](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/321972e2-e8e1-4d36-a8da-015dabaf1959)

전원 스위치도 예쁘고 아주 마음에 든다.

![IMG_0036](https://github.com/GonoBae/GonoBae.github.io/assets/87271529/b3f1839a-d3d2-4529-ad7b-8469257216e0)

SSD 에 Ubuntu 24.04 를 설치 후 모니터 연결 후 부팅에 성공하였다.

