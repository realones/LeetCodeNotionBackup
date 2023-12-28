# Sqrt(x)

#: 69
Difficult: Easy
Tags: BS_Idx, Math
link: https://leetcode.com/problems/sqrtx/
Priority: Low
Created time: September 7, 2022 12:12 PM
Last edited time: October 28, 2023 10:00 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算高BS&SwipeLine

Given a non-negative integer `x`, compute and return *the square root of* `x`.

Since the return type is an integer, the decimal digits are **truncated**, and only **the integer part** of the result is returned.

**Note:** You are not allowed to use any built-in exponent function or operator, such as `pow(x, 0.5)` or `x ** 0.5`.

**Example 1:**

```
Input: x = 4
Output: 2

```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```

**Constraints:**

- `0 <= x <= 2^31 - 1`

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l=0, r=x;
        while(l<r){
            int m=l+((long)r-l+1)/2;
            if(m>x/m){r=m-1;}
            else{l=m;}
        }
        return r;
    }
};
```