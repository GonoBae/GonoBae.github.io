---
layout: single
title: "[코딩테스트] 세 번째 : 동전"
categories: CordingTest
tag: CordingTest
---

### <문제>

동전이 총 N 종류, 각각 여러개 가지고 있음 <br>
N종류의 동전을 적절히 사용해서 합을 K로 만들려고 한다. 필요한 동전 개수의 최솟 값은?

#### 입력 : 첫째 줄 N, K

####        둘째 줄 N개의 줄에 동전의 가치 A(i)가 오름차순으로 주어짐 

####        (A1 =1, i >= 2인 경우 A(i)는 A(i-1)의 배수)

#### 출력 : K를 만드는데 필요한 동전 개수의 최솟값



우선... <br>
K를 맞추기 위한 최솟값이니 K보다 바로 아래에 있는 값으로 감산해 나가면 될 것 같다.



### [C#]

```c#
using System;

class Program
{
    static void Main()
    {
        int answer = 0;
        string[] input = Console.ReadLine().Split(' ');
        int coins = int.Parse(input[0]);	// 코인수 N
        int goal = int.Parse(input[1]);		// 목표 값 K
        int[] coinsValue = new int[coins];	// 코인들의 값
		
        // 입력 받은 값을 배열에 저장
        for(int idx = 0; idx < coins; idx++)
        {
            coinsValue[idx] = int.Parse(Console.ReadLine());
        }
        
        // 큰 값을 먼저 찾아서 감산해야 하기 때문에
        // 높은 순서로 재정렬 
        Array.Reverse(coinsValue);

        while (goal != 0)
        {
            for (int idx = 0; idx < coinsValue.Length; idx++)
            {
                if (coinsValue[idx] <= goal)
                {
                    goal -= coinsValue[idx];
                    answer++;
                    idx--;	// 다시 검사하기 위함
                }
            }
        }
        Console.WriteLine(answer);
    }
}
```

### [C++]

```c++
#include <iostream>
#include <vector>    
#include <algorithm> 

using namespace std;

int main()
{
    int answer = 0;
    int coins, goal;
    cin >> coins >> goal;

    vector<int> coinsValue(coins);
    
    for (int idx = 0; idx < coinsValue.size(); idx++)
    {
        cin >> coinsValue[idx];
    }
    
    // 시작과 끝부분을 지정하여 리버스 시킴
    reverse(coinsValue.begin(), coinsValue.end());
    
    while (goal != 0) 
    {
        for(int idx = 0; idx < coinsValue.size(); idx++)
        {
            if (goal >= coinsValue[idx]) 
            {
                goal -= coinsValue[idx];
                idx--;
                answer++;
            }
        }
    }
    cout << answer;
    return 0;
}
```

vector 라고 STL에 있는 컨테이너라고 한다. <br>
배열과 같은 자료구조 인가 보다.
