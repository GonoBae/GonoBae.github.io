---
layout: single
title: "[코딩테스트] 여섯 번째 : 보물"
categories: CordingTest
tag: CordingTest
---

![image-20220427214535320](../images/2022-04-27-CordingTest6/image-20220427214535320.png)



###### 최솟값이 되기 위해서는 '-' 다음에 '()'를 붙여서 양수를 음수로 바꿔버리는 방법이 있다. 

즉, 첫 예제 55-50+40 에서 55-(50+40)을 하면 최솟값이된다.



'-'가 있을때와 없을때를 나누어 Split()을 한다 (부호는 사라짐 = 기본값 '+')

또, 내부에서 '+' 기준으로 나누어 int형으로 바꿔 연산을 해야한다.

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
            string input = Console.ReadLine();
            string[] nums;

            if (input.Contains('-'))
            {
                nums = input.Split('-');

                for (int idx = 0; idx < nums.Length; idx++)
                {
                    if (nums[idx].Contains('+'))
                    {
                        string[] strPlusNums = nums[idx].Split('+');
                        int[] plusNumss = Array.ConvertAll(
                            strPlusNums, int.Parse);
                        foreach (int num in plusNumss)
                        {
                            if (idx != 0)
                            {
                                answer -= num;
                            }
                            else
                            {
                                answer += num;
                            }
                        } 
                    }
                    else
                    {
                        // 50 -40 + 10
                        // 50 
                        // 40 + 10
                        // +50 
                        if (idx != 0)
                        {
                            answer -= int.Parse(nums[idx]);
                        }
                        else
                        {
                            answer += int.Parse(nums[idx]);
                        }
                    }
                }
            }
            else
            {
                nums = input.Split('+');
                for (int idx = 0; idx < nums.Length; idx++)
                {
                    answer += int.Parse(nums[idx]);
                }
            }
            Console.WriteLine(answer);
        }
    }
}
```

![image-20220427235238111](../../images/2022-04-27-CordingTest6/image-20220427235238111.png)



### [C++]