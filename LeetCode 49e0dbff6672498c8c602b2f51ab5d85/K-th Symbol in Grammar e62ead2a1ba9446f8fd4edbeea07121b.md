# K-th Symbol in Grammar

#: 779
Difficult: Medium
Tags: Bit Manipulation, Math, recursion
link: https://leetcode.com/problems/k-th-symbol-in-grammar
Priority: High
Status: Dig More, No Idea At First
Created time: October 28, 2023 1:51 AM
Last edited time: October 28, 2023 12:32 PM
source: leetcode_daily

We build a table of `n` rows (**1-indexed**). We start by writing `0` in the `1st` row. Now in every subsequent row, we look at the previous row and replace each occurrence of `0` with `01`, and each occurrence of `1` with `10`.

- For example, for `n = 3`, the `1st` row is `0`, the `2nd` row is `01`, and the `3rd` row is `0110`.

Given two integer `n` and `k`, return the `kth` (**1-indexed**) symbol in the `nth` row of a table of `n` rows.

**Example 1:**

```
Input: n = 1, k = 1
Output: 0
Explanation: row 1:0
```

**Example 2:**

```
Input: n = 2, k = 1
Output: 0
Explanation:
row 1: 0
row 2:01

```

**Example 3:**

```
Input: n = 2, k = 2
Output: 1
Explanation:
row 1: 0
row 2: 01
```

**Constraints:**

- `1 <= n <= 30`
- `1 <= k <= 2n - 1`

Solution:

```cpp
class Solution {
public:
    int kthGrammar(int n, int k) {
        return helper(k-1);//0 based
    }

    int helper(int k){
        if(k==0) return 0;
        int len=1;//len to cut
        while(len*2<k+1) len*=2;
        return !helper(k-len);//since everytime double, the 2nd half bits are flipped
    }
};

/*
0
01
0110
0110 1001
0110 1001 1001 0110
0110 1001 1001 0110 1001 0110 0110 1001

X ->
X reverseX

1->2->4->8
*/
```