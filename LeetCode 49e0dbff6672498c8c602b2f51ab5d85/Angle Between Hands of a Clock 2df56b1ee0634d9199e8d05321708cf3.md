# Angle Between Hands of a Clock

#: 1344
Difficult: Medium
Tags: Math
link: https://leetcode.com/problems/angle-between-hands-of-a-clock/description/
Priority: Low
Created time: December 8, 2023 3:54 PM
Last edited time: December 8, 2023 3:54 PM
source: facebookFreq

Given two numbers, `hour` and `minutes`, return *the smaller angle (in degrees) formed between the* `hour` *and the* `minute` *hand*.

Answers within `10-5` of the actual value will be accepted as correct.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/12/26/sample_1_1673.png](https://assets.leetcode.com/uploads/2019/12/26/sample_1_1673.png)

```
Input: hour = 12, minutes = 30
Output: 165

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/12/26/sample_2_1673.png](https://assets.leetcode.com/uploads/2019/12/26/sample_2_1673.png)

```
Input: hour = 3, minutes = 30
Output: 75

```

**Example 3:**

![https://assets.leetcode.com/uploads/2019/12/26/sample_3_1673.png](https://assets.leetcode.com/uploads/2019/12/26/sample_3_1673.png)

```
Input: hour = 3, minutes = 15
Output: 7.5

```

**Constraints:**

- `1 <= hour <= 12`
- `0 <= minutes <= 59`

```cpp
class Solution {
public:
    double angleClock(int hour, int minutes) {
        // hour position:
        //     base:
        //         360*hour/12
        double hourBase=360*(hour%12)*1.0/12;
        //     +
        //     changeByMnit:
        //         360 * (1/12 * min/60)
        double hourChange=360*(1.0/12 * minutes/60);
        double h=hourBase+hourChange;
        // min position:
        //     base:
        //         360 * min/60
        double m=360*minutes*1.0/60;
        // return diff
        return min(abs(h-m), 360-abs(h-m));
    }
};

/*
hour position:
    base:
        360*hour/12
    +
    changeByMnit:
        360 * (1/12 * min/60)
min position:
    base:
        360 * min/60
return diff
*/
```