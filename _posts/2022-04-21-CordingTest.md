---

layout: single 
title: "[코딩테스트] 첫 번째 : 설탕 배달
categories: CordingTest
tag: C#, C++

---

한 문제를 가지고 C#과 C++을 이용해 풀어 나갈 것이다.

그럼 Start !



문제 :

입력 값 : N

출력 값 : 3kg 봉지와 5kg 봉지의 합

조건 : N을 가지고 최대한 적은 수의 봉지를 들려고한다.

 		  만약 N이 14면 5kg 2개 3kg 1개를 들 수 있지만 1kg이 남아서 출력 값은 -1이 된다.



첫 시도

a = 3kg의 수, b= 5kg의 수

5kg의 봉지의 수를 먼저 구하고 3kg의 봉지의 수를 구해야 한다.

((N % 5b) % 3a) == 0
출력 값 : a+b

 ((N % 5b) % 3a) == !0
출력 값 : -1

a와 b 구하는 법
b = N / 5
a = (N % 5) / 3

N % 0 의 연산이 불가능 하다고 에러 발생



두 번째 시도

N에서 감산식을 적용해보자

6은 3으로 2번이 가능한데

5로 빼버리니까 결과값이 -1이 됬다



세 번째 시도

5와 3의 배수로 먼저 탐색을 해봐야 할 것 같다

생각해보니 5와 3의 배수가 아니면 모두 -1이라는 값이 나온다
그렇다면 5의 배수인지 확인하고
아니라면 3의 배수로 확인한 뒤 결과를 내면 될것같다
a,b를 나눌필요가 없기도 하다 합쳐버리자



###  [C#]

```c#
using System;

class Program
{
    static void Main()
    {
        int input = int.Parse(Console.ReadLine());
        int result = 0;

        while(input > 0)
        {
            if (input % 5 == 0)
            {
                input -= 5;
                result++;
            }
            else if (input % 3 == 0)
            {
                input -= 3;
                result++;
            }
            else if (input > 5)
            {
                input -= 5;
                result++;
            }
            else
            {
                result = -1;
                break;
            }
        }
        Console.WriteLine(result);
    }
}
```



### [C++]

```c++
#include<iostream>

using namespace std;

int main()
{
    int input;
    int result = 0;
    
    cin >> input;
    
    while(input > 0)
    {
        if (input % 5 == 0)
        {
            input -= 5;
            result++;
        }
        else if (input % 3 == 0)
        {
            input -= 3;
            result++;
        }
        else if (input > 5)
        {
            input -= 5;
            result++;
        }
        else 
        {
            result = -1;
            break;
        }
    }
    cout << result;
    return 0;
}
```



아직 알고리즘 적으로 C#과 C++에 차이는 없었다.

유니티를 하다보니 C#과 C++의 문법이 너무 헤깔린다.

다시 차근차근 알아가야 할 것 같다.