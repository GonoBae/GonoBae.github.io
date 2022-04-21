---
layout: single
title: "[코딩테스트] 두 번째 : ATM"
categories: CordingTest
tag: CordingTest
---



### <문제>

입력 값 : N = 사람의 수, P(i) = 각 사람이 인출하는데 필요 시간

출력 값 : 각 사람이 돈을 인출하는데 필요한 시간의 합의 최솟 값


P(i)를 작은 수부터 차례로 나열하면 최솟 값이 될것같다

합산은 사람 수(N) 만큼 진행한다

결과론 적으로 보면
  0번째는 N 개
  1번째는 N-1 개
  2번째는 N-2 개
        ... 
  N-1번째는 N-(N-1) 개
가 되어 [출력 값] = N * P0 + (N-1) * P1 + (N - (N-1)P(N-1) 라는 식이 도출 된다.



### [C#]

```
class Program
{
    static void Main()
    {
        int people = int.Parse(Console.ReadLine());
        string timeString = Console.ReadLine();
        string[] times = timeString.Split(' ');

        int answer = 0;

        Array.Sort(times);

        for(int idx = 0; idx < people; idx++)
        {
            answer += (people - idx) * int.Parse(times[idx]);
        }

        Console.WriteLine(answer);
    }
}
```

맞는거 같은데 왜 틀렸지..? 



### [C++]

위에꺼 해결하고 해보자...