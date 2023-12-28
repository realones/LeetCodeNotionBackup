# Determine the Minimum Sum of a k-avoiding Array

#: 2829
Difficult: Medium
Tags: Greedy, Hash
link: https://leetcode.com/problems/determine-the-minimum-sum-of-a-k-avoiding-array/
Priority: Low
Created time: August 19, 2023 9:35 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests
related: Find the Minimum Possible Sum of a Beautiful Array (Find%20the%20Minimum%20Possible%20Sum%20of%20a%20Beautiful%20Array%20c6e3dda8b1ad4ebfbdc2eb54d5833100.md)

You are given two integers, `n` and `k`.

An array of **distinct** positive integers is called a **k-avoiding** array if there does not exist any pair of distinct elements that sum to `k`.

Return *the **minimum** possible sum of a k-avoiding array of length* `n`.

**Example 1:**

```
Input: n = 5, k = 4
Output: 18
Explanation: Consider the k-avoiding array [1,2,4,5,6], which has a sum of 18.
It can be proven that there is no k-avoiding array with a sum less than 18.

```

**Example 2:**

```
Input: n = 2, k = 6
Output: 3
Explanation: We can construct the array [1,2], which has a sum of 3.
It can be proven that there is no k-avoiding array with a sum less than 3.

```

**Constraints:**

- `1 <= n, k <= 50`

```cpp
class Solution {
public:
    int minimumSum(int n, int k) {
        unordered_set<int> ignore;
        int res=0;
        int x=1;
        for(int i=0; i<n; i++){
            res+=x;
            ignore.insert(k-x);
            x++;
            while(ignore.count(x)) x++;
        }
        return res;
    }
};

//for each x, from 1,2,..., will generate ignore == k-x, if x==ignore, skip i
```