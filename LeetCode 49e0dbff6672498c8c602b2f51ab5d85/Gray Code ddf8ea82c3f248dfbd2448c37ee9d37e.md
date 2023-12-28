# Gray Code

#: 89
Difficult: Medium
Tags: Bit Manipulation, DP, Greedy, Math
link: https://leetcode.com/problems/gray-code/
Priority: Medium
Status: More Solution, NotUnderstand, toRevisit
Created time: September 24, 2023 6:43 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

An **n-bit gray code sequence** is a sequence of `2n` integers where:

- Every integer is in the **inclusive** range `[0, 2n - 1]`,
- The first integer is `0`,
- An integer appears **no more than once** in the sequence,
- The binary representation of every pair of **adjacent** integers differs by **exactly one bit**, and
- The binary representation of the **first** and **last** integers differs by **exactly one bit**.

Given an integer `n`, return *any valid **n-bit gray code sequence***.

**Example 1:**

```
Input: n = 2
Output: [0,1,3,2]
Explanation:
The binary representation of [0,1,3,2] is [00,01,11,10].
- 00 and 01 differ by one bit
-01 and11 differ by one bit
- 11 and 10 differ by one bit
-10 and00 differ by one bit
[0,2,3,1] is also a valid gray code sequence, whose binary representation is [00,10,11,01].
-00 and10 differ by one bit
- 10 and 11 differ by one bit
-11 and01 differ by one bit
- 01 and 00 differ by one bit

```

**Example 2:**

```
Input: n = 1
Output: [0,1]

```

**Constraints:**

- `1 <= n <= 16`

comment:

我用greedy直接做出来了，可是这样greedy真的没问题吗，不会走到死路吗？

似乎有Solutions有更好的解

```cpp
class Solution {
public:
    vector<int> grayCode(int n) {
        int M=1<<n;//0~M-1
        unordered_set<int> visited;
        vector<int> res(M,0);
        visited.insert(0);
        function nextAvailable = [&](int num)->int{
            for(int d=0;d<n;d++){
                int nxt=num^(1<<d);
                if(visited.count(nxt)) continue;
                return nxt;
            }
            std::cout << "not able to find" << std::endl;
            return 0;
        };
        for(int i=1; i<M; i++){
            int pre=res[i-1];
            int cur=nextAvailable(pre);
            res[i]=cur;
            visited.insert(cur);
        }
        return res;
    }
};

/*
1st elem =0
each elem appear <=1 time
adj elems, have exact 1 bit change

return: any valid seq
*/
```