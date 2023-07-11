---
layout: single
title: "VSCode 에서 Library 사용하기"
categories:
  - CppStudy
tags:
  - [C++, Library, VSCode, StaticLibrary]
toc: true
toc_sticky: true

Date: 2023-06-09
published: true
---

# VSCode 에서 정적 라이브러리 사용하기
## 정적 라이브러리란?
우리가 사용하는 C++ 코드는 컴파일 과정을 거쳐 실행파일이 나오게 됩니다.

이 컴파일 과정은 간단하게 `Preprocessing -> Compilation -> Assemble -> Linking` 이다.

Assemble 단계에서 생성된 Object file 을 Linking 단계에서 필요한 라이브러리들과 연결시켜 하나의 실행파일을 만든다.

이 때 연결되는 라이브러리가 Static Library 인데 실행파일의 일부가 된다.

VS Code 에서는 우리가 직접 라이브러리를 연결시켜 주어야 한다.

## cmake
CMake 는 빌드 파일을 생성해주는 프로그램이다.

VS Code 에서는 우리가 원하는 라이브러리를 자동으로 Link 해주지 못하기 때문에

CMake 를 통해 Link 해주고 빌드 파일을 생성한다.

## Linking
다음과 같은 프로젝트가 있다.

<img width="233" alt="1" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/32542cbd-8603-48d2-bb1e-cb189cb6db33">

Server 스크립트에서 ServerLib 폴더의 Lib 라이브러리를 가져와 사용할 것이다.

Lib.h
```cpp
void hello_lib();
```

Lib.cpp
```cpp
#include "Lib.h"
#include <iostream>

void hello_lib() {
    std::cout << "Hello, I'm Lib" << std::endl;
}
```

server.cpp
```cpp
#include <iostream>
#include "../ServerLib/Lib.h"

int main() {
    hello_lib();
    return 0;
}
```

이렇게 작성 후 빌드를 하면 오류가 발생한다.

`linker command failed` 라고 Linking 단계에서 실패한 것을 볼 수 있다.

이를 CMake 로 해결해보자.

CMake 를 통해 Linking 을 할 때 우리가 작성해야 할 스크립트가 있다.

바로 `CMakeLists.txt` 이다.

실행할 스크립트가 있는 폴더 (Server) 로 가서 `CMakeLists.txt` 파일을 만든다.

이제 터미널을 열고 Server 폴더 (CMakeLists.txt 파일이 있는 경로) 로 이동한다.

<img width="560" alt="1" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/112b2ae8-5b14-41ca-beef-643f514a521a">

Server 폴더로 이동 후 `cmake .` 명령어를 실행시키면 다음과 같이 진행되고

<img width="212" alt="2" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/03601c4c-7651-4f33-b5e0-37cba9e179ee">

Server 폴더에 많은 파일들이 생긴다.

자동완성을 사용하여 CMakeLists 파일을 작성해보자.

```cpp
# 최소버전 입력
cmake_minimum_required(VERSION 3.0.0)
# 프로젝트 정보
# 프로젝트 버전 / 사용하는 언어 (C = C , CXX = C++)
project(Server VERSION 0.1.0 LANGUAGES C CXX)

# 실행 파일 이름 + 해당 실행 파일을 만드는데 필요한 소스
add_executable(Server Server.cpp)
# Lib.cpp 로 정적 라이브러리 Lib 을 만든다.
add_library(Lib STATIC ../Serverlib/Lib.cpp)
# Server 파일에 Lib 을 링크
target_link_libraries(Server Lib)
```
준비를 모두 마쳤으니 `make` 명령어로 빌드파일을 생성해보자.

<img width="560" alt="3" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/ecc650b5-60e1-4d09-9cc6-2131008324fd">

생성한 Server 파일을 `./Server` 명령어로 실행시키면 정상작동 하는 것을 볼 수 있다.

<img width="242" alt="4" src="https://github.com/GonoBae/GonoBae.github.io/assets/87271529/17bbd4f2-f61a-4e24-bfb3-5d6700afe5b3">

끝!