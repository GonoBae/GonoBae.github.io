---
layout: single
title: "Stack"
categories:
  - Algorithm
tags:
  - [BOJ, Stack, Algorithm, C++]
---

# Stack에 대해 알아보자.

> <span style="color:yellow"> **Stack** </span>
> - LIFO (Last In First Out) 로써 먼저 들어간 데이터가 늦게 나오게 되는 구조이다. <br>
> - #include&#60;stack&#62; 으로 사용 가능하며 push , pop , top , empty , size 함수를 가진다.

![1](https://user-images.githubusercontent.com/87271529/168085166-d4e82515-bd97-4228-a15a-4b835d0729dd.jpeg)

- push(element) : top 에 원소를 추가한다.
- pop() : top 에 있는 원소를 삭제한다.
- top() : stack의 가장 위에 있는 원소를 반환한다.
- empty() : 스택이 비어있으면 true, 아니면 false 반환
- size() : 스택 사이즈를 반환

백준에 나와있는 10828문제를 통해 이를 구현해보자.

> 문제링크 [Q_10828](https://www.acmicpc.net/problem/10828)

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int arr[10001], cnt = 0;
void push(int x)
{
    arr[cnt++] = x;
}
int pop()
{
    if(cnt == 0) return -1;
    return arr[--cnt]; 
}
int top()
{
    if(cnt == 0) return -1;
    return arr[cnt - 1];
}
int size()
{
    return cnt;
}
int empty()
{
    if(cnt == 0) return 1;
    return 0;
}
int main()
{
    int N;
    cin >> N;
    for(int i = 0; i < N; i++)
    {
        char cmd[30];
        cin >> cmd;
        if(strcmp(cmd, "push") == 0)
        {
            int t;
            cin >> t;
            push(t);
        }
        else if(strcmp(cmd, "pop") == 0)
            cout << pop() << endl;
        else if(strcmp(cmd, "top") == 0)
            cout << top() << endl;
        else if(strcmp(cmd, "empty") == 0)
            cout << empty() << endl;
        else if(strcmp(cmd, "size") == 0)
            cout << size() << endl;
    }
    return 0;
}
```

> stack 관련 문제
