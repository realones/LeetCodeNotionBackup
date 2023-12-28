# Power of Four

#: 342
Difficult: Easy
Tags: Bit Manipulation, Math
link: https://leetcode.com/problems/power-of-four/
Priority: Low
Created time: October 23, 2023 1:48 PM
Last edited time: October 23, 2023 2:00 PM
source: leetcode_daily

Given an integer `n`, return *`true` if it is a power of four. Otherwise, return `false`*.

An integer `n` is a power of four, if there exists an integer `x` such that `n == 4x`.

**Example 1:**

```
Input: n = 16
Output: true

```

**Example 2:**

```
Input: n = 5
Output: false

```

**Example 3:**

```
Input: n = 1
Output: true

```

**Constraints:**

- `231 <= n <= 231 - 1`

**Follow up:**

Could you solve it without loops/recursion?

Solution 1:

```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        if(n<=0) return false;
        while(n%4==0) n/=4;
        return n==1;
    }
}
```

Solution bit manipulate:

```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        if(n<=0) return false;
        while(n){
            if(n==1) return true;
            if(n&1) return false;
            if(n&2) return false;
            n>>=2;
        }
        return true;
    }
}
```