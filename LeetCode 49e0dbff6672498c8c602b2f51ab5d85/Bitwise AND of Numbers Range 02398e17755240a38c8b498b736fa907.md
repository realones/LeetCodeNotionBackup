# Bitwise AND of Numbers Range

#: 201
Difficult: Medium
Tags: Bit Manipulation
link: https://leetcode.com/problems/bitwise-and-of-numbers-range/
Priority: Low
Created time: June 5, 2023 12:55 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given two integers `left` and `right` that represent the range `[left, right]`, return *the bitwise AND of all numbers in this range, inclusive*.

**Example 1:**

```
Input: left = 5, right = 7
Output: 4

```

**Example 2:**

```
Input: left = 0, right = 0
Output: 0

```

**Example 3:**

```
Input: left = 1, right = 2147483647
Output: 0

```

**Constraints:**

- `0 <= left <= right <= 231 - 1`

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int res=0;
        for(int i=31; i>=0; i--){
            int l=(left>>i)&1;
            int r=(right>>i)&1;
            if(!l && !r) continue;
            else if(l && r) res+=(1<<i);
            else if(l || r) return res;
        }
        return res;
    }
};
```