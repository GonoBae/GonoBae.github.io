---
layout: single
title: "Priority Queue"
categories: "Algorithm"
tags:
  [BOJ , Algorithm, Priority Queue, Queue]
---

# C++ Priority_Queue

<span style="color:yellow"> 우선순위 큐(Priority Queue) 란? </span> <br>
정렬이 포함된 Queue 라고 생각하면 된다.
`#include <queue>` 헤더파일을 사용하며, <br>
n 개의 원소를 정렬할 때 O(nlogn) 의 시간복잡도를 갖는다.
내부Heap이라는 자료구조를 사용한다.
기본적으로 가장 큰 값이 Top 을 유지하고, 내림차순으로 정렬되게끔 설계되어 있다.

```Cpp
#include <iostream>
#include <queue>
using namespace std;
int main()
{
    priority_queue<int> PQ;
    PQ.push(2);
    PQ.push(1);
    PQ.push(5);
    PQ.push(3);
    PQ.push(7);
    PQ.push(6);
    while(!PQ.empty())
    {
        cout << PQ.top() << ' ';
        PQ.pop();
    }
    return 0;
}
```

> 결과값
![Result1](https://user-images.githubusercontent.com/87271529/167148484-ea6eaff7-23fc-483c-8d70-4ea6a1bf3d1a.png)

