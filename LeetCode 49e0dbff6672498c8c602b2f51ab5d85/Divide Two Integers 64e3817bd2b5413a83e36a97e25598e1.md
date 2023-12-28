# Divide Two Integers

#: 29
Difficult: Medium
Tags: Bit Manipulation, Math
link: https://leetcode.com/problems/divide-two-integers/
Priority: Low
Created time: July 8, 2022 4:26 PM
Last edited time: October 12, 2023 2:56 PM
source: jiuzhang, 算初BS

Given two integers `dividend` and `divisor`, divide two integers **without** using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, `8.345` would be truncated to `8`, and `-2.7335` would be truncated to `-2`.

Return *the **quotient** after dividing* `dividend` *by* `divisor`.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: `[−231, 231 − 1]`. For this problem, if the quotient is **strictly greater than** `231 - 1`, then return `231 - 1`, and if the quotient is **strictly less than** `-231`, then return `-231`.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = 3.33333.. which is truncated to 3.

```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = -2.33333.. which is truncated to -2.

```

**Constraints:**

- `231 <= dividend, divisor <= 231 - 1`
- `divisor != 0`

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN && divisor == -1) {
            return INT_MAX;
        }
        long dvd = labs(dividend), dvs = labs(divisor), ans = 0;
        int sign = dividend > 0 ^ divisor > 0 ? -1 : 1;
        while (dvd >= dvs) {
            long temp = dvs, m = 1;
            while (temp << 1 <= dvd) {
                temp <<= 1;
                m <<= 1;
            }
            dvd -= temp;
            ans += m;
        }
        return sign * ans;
    }
};

/*
//if allow mutiplication
class Solution {
public:
    int divide(int dividend, int divisor) {
        if(divisor==0){return INT_MAX;}
        if(dividend==INT_MIN && divisor==-1){return INT_MAX;}
        bool positive=!((dividend>0)^(divisor>0));
        long long dvd = labs(dividend);
        long long dvs = labs(divisor);
        long long l=0,r=dvd;
        while(l<r){
            long long m=l+(r-l+1)/2;
            if(dvs*m > dvd){r=m-1;}
            else{l=m;}
        }
        return positive?r:-r;
    }
};
*/
```