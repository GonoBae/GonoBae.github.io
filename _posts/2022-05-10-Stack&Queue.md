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
> - `#include&#60;stack&#62;` 으로 사용 가능하며 `push , pop , top , empty , size` 함수를 가진다.

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

이 문제에서 stack 과 함께 공부할 수 있는 것은 문자열에 대한 것이다.
`#include <cstring>` 헤더에서 사용할 수 있는 `strcmp` 함수이다.
문자열을 비교하는 이 함수는 다음 예시처럼 쓰인다.

> char c[] = "ABCDEFG"; <br>
> char d[] = "ABCDEFG"; <br>
> strcmp(c, d);         // --> 문자열이 <u>같으므로</u> <span style="color:yellow"> 0 을 반환 </span> <br>
> strcmp(c, "BBCDEFG"); // --> c 와 "BBCDEFG" 의 <u>첫 번째 문자가 다름.</u> <span style="color:yellow"> A < B 이므로 음수 반환 </span> <br>
> strcmp(c, "AACDEFG"); // --> c 와 "AACDEFG" 의 <u>두 번째 문자가 다름.</u> <span style="color:yellow"> B > A 이므로 양수 반환 </span> <br>

```cpp
#include <iostream>
#include <cstring>
using namespace std;
int main()
{
    int check;
    char c[] = {"ABCDEFG"};
    char d[] = {"ABCDEFG"};
    check = strcmp(c, d);
    cout << check << ' ';
    check = strcmp(c, "BBCDEFG");
    cout << check << ' ';
    check = strcmp(c, "AACDEFG");
    cout << check << ' ';
    return 0;
}
```

> stack 관련 문제[Q_2493](https://www.acmicpc.net/problem/2493)
