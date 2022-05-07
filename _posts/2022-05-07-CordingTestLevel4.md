---
layout: single
title: "[코딩테스트] Level4 : 1차원 배열"
categories: CordingTest
tag: CordingTest
---

총 7문제 였다

조금 힘들었던 문제가 있었음으로 해당 코드는 올려본다.

### [C++]

> for-each 사용

C#과는 생긴게 조금 다르니 조심하자

```C++
// for (type : Arr)
for (char ch : strSum)
    {
        count[ch - '0']++;
    }
```

- for-each 문으로 값을 바꾸려면 &(ch)참조를 해야 한다.
- 동적 할당에는 사용 할 수 없으니 vector를 이용해야 한다.

#### 4344 : 평균은 넘겠지
```C++
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    cout << fixed;
    cout.precision(3);

    int cases;
    cin >> cases;

    vector<float> answer(cases);

    for (int idx = 0; idx < cases; idx++)
    {
        int people;
        int sum = 0, count = 0;
        float avg = 0;

        cin >> people;
        vector<int> scores(people);

        for (int idx = 0; idx < people; idx++)
        {
            cin >> scores[idx];
            sum += scores[idx];
        }

        avg = sum / people;
        
        for (int idx = 0; idx < people; idx++)
        {
            if (avg < scores[idx])
            {
                count++;
            }
        }

        answer[idx] = (float)(count * 100 / people);
    }
 
    for (float result : answer)
    {
        cout << result << "%" << "\n";
    }

    return 0;
}
```

아무튼 이번에도 완료 !!

![FourthLevel](../../images/2022-05-07-CordingTestLevel4/FourthLevel.PNG)
