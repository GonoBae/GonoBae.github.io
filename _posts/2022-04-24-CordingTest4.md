---
layout: single
title: "[코딩테스트] 네 번째 : 회의실"
categories: CordingTest
tag: CordingTest
---



### <문제>

#### 입력 : [첫째 줄 :  회의 수  N] [ 둘째 줄 ~ N+1까지 : 각 회의 시작과 끝나는 시간]

#### 출력 : [회의의 최대 개수]

우선 끝나는 시간을 기준으로 오름차순 정렬하자

그리고 1 ~ 3, 3 ~ 3 과 같은 예외가 있기 때문에

시작 시간을 기준으로 오름차순으로 한번더 정렬해야한다.

그 다음부터 i의 끝나는 시간과  i+1의 시작 시간을 판별하면 될 것 같다.

+ 끝나는 시간을 기준으로 정렬했기 때문에 맨 처음 오는 것은 무조건 카운트 된다고 봐도 무방하다.

### [C#]

```c#
using System;
using System.Linq;

namespace ConsoleApp1
{
    class Program
    {
        static void Main()
        {
            int answer = 0;
            int rooms = int.Parse(Console.ReadLine());  
            int currentTime = 0;
			
            // 튜플타입의 배열
            (int start, int end)[] meetingTime = new (int, int)[rooms];

            for (int idx = 0; idx < rooms; idx++)
            {
                string timeString = Console.ReadLine();
                string[] InputTimes = timeString.Split();
                
                meetingTime[idx].start = int.Parse(InputTimes[0]);
                meetingTime[idx].end = int.Parse(InputTimes[1]);
            }
            
			// end 시간기준 오름정렬 후 start 시간기준 재정렬  
            meetingTime = meetingTime.OrderBy(x => x.end).ThenBy(x => x.start).ToArray();

            for (int idx = 0; idx < rooms; idx++)
            {
                if (meetingTime[idx].start >= currentTime)
                {
                    currentTime = meetingTime[idx].end;
                    answer++;
                }
            }
            Console.WriteLine(answer);
        }
    }
}
```

![image-20220424193337281](../../images/2022-04-24-CordingTest4/image-20220424193337281.png)

 Tuple과 Linq의 Orderby,ThenBy에 대해 처음 사용해보았다.

까먹지 말자... 



### [C++]

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(pair<int, int> _a, pair<int, int> _b)
{
    if (_a.second == _b.second) { return _a.first < _b.first; }
    return _a.second < _b.second;
}

int main()
{
    int answer = 0;
    int rooms;
    int start, end;
    int currentTime = 0;
    cin >> rooms;

    vector<pair<int, int>> meetingTime(rooms);

    for (int idx = 0; idx < meetingTime.size(); idx++)
    {
        cin >> meetingTime[idx].first >> meetingTime[idx].second;
    }
    
    // 정렬 기준 재정의
    sort(meetingTime.begin(), meetingTime.end(), compare);

    for (int idx = 0; idx < rooms; idx++)
    {
        if (meetingTime[idx].first >= currentTime)
        {
            currentTime = meetingTime[idx].second;
            answer++;
        }
    }
    cout << answer;
    return 0;
}
```

![image-20220424193317083](../../images/2022-04-24-CordingTest4/image-20220424193317083.png)

자료 구조 타입명이나 매소드들의 이름과 사용법이 C#과 조금씩 차이가 있으니 잘 구분해보자