# Minimum Flips to Make a OR b Equal to c

#: 1318
Difficult: Medium
Tags: Bit Manipulation
link: https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/
Priority: Pass
Created time: June 6, 2023 10:38 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, leetcode_daily

Given 3 positives numbers `a`, `b` and `c`. Return the minimum flips required in some bits of `a` and `b` to make ( `a` OR `b` == `c` ). (bitwise OR operation).

Flip operation consists of change **any** single bit 1 to 0 or change the bit 0 to 1 in their binary representation.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/06/sample_3_1676.png](https://assets.leetcode.com/uploads/2020/01/06/sample_3_1676.png)

```
Input: a = 2, b = 6, c = 5
Output: 3
Explanation:After flips a = 1 , b = 4 , c = 5 such that (a ORb ==c)
```

**Example 2:**

```
Input: a = 4, b = 2, c = 7
Output: 1

```

**Example 3:**

```
Input: a = 1, b = 2, c = 3
Output: 0

```

**Constraints:**

- `1 <= a <= 10^9`
- `1 <= b <= 10^9`
- `1 <= c <= 10^9`

```cpp
class Solution {
public:
    int minFlips(int a, int b, int c) {
        int res=0;
        for(int i=0; i<32; i++){
            int ai=(a>>i)&1;
            int bi=(b>>i)&1;
            int ci=(c>>i)&1;
            if(ci==0){
                res+= ai + bi;
            }
            else{
                res+= 1- (ai | bi);
            }
        }
        return res;
    }
};
```