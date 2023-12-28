# Counting Bits

#: 338
Difficult: Easy
Tags: BinaryLifting, Bit Manipulation, DP
link: https://leetcode.com/problems/counting-bits/description/
Priority: High
Status: HardToImplement, No Idea At First
Created time: July 4, 2023 6:40 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given an integer `n`, return *an array* `ans` *of length* `n + 1` *such that for each* `i` **(`0 <= i <= n`)*,* `ans[i]` *is the **number of*** `1`***'s** in the binary representation of* `i`.

**Example 1:**

```
Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10

```

**Example 2:**

```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101

```

**Constraints:**

- `0 <= n <= 105`

**Follow up:**

- It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
- Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> res(n+1);
        for(int i=0; i<=n; i++){
            res[i]=res[i>>1] + (i&1);
        }   
        return res;
    }
}
```