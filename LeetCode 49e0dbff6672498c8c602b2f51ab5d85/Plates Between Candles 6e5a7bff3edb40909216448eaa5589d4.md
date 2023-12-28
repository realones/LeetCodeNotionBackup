# Plates Between Candles

#: 2055
Difficult: Medium
Tags: Array, BS_todo, Prefix, string
link: https://leetcode.com/problems/plates-between-candles/description/
Priority: Medium
Status: More Solution
Created time: November 23, 2023 3:49 AM
Last edited time: November 23, 2023 3:50 AM
source: AmazonFreq

There is a long table with a line of plates and candles arranged on top of it. You are given a **0-indexed** string `s` consisting of characters `'*'` and `'|'` only, where a `'*'` represents a **plate** and a `'|'` represents a **candle**.

You are also given a **0-indexed** 2D integer array `queries` where `queries[i] = [lefti, righti]` denotes the **substring** `s[lefti...righti]` (**inclusive**). For each query, you need to find the **number** of plates **between candles** that are **in the substring**. A plate is considered **between candles** if there is at least one candle to its left **and** at least one candle to its right **in the substring**.

- For example, `s = "||**||**|*"`, and a query `[3, 8]` denotes the substring `"*||*****|"`. The number of plates between candles in this substring is `2`, as each of the two plates has at least one candle **in the substring** to its left **and** right.

Return *an integer array* `answer` *where* `answer[i]` *is the answer to the* `ith` *query*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/10/04/ex-1.png](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

```
Input: s = "**|**|***|", queries = [[2,5],[5,9]]
Output: [2,3]
Explanation:
- queries[0] has two plates between candles.
- queries[1] has three plates between candles.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/10/04/ex-2.png](https://assets.leetcode.com/uploads/2021/10/04/ex-2.png)

```
Input: s = "***|**|*****|**||**|*", queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
Output: [9,0,0,0,0]
Explanation:
- queries[0] has nine plates between candles.
- The other queries have zero plates between candles.

```

**Constraints:**

- `3 <= s.length <= 105`
- `s` consists of `'*'` and `'|'` characters.
- `1 <= queries.length <= 105`
- `queries[i].length == 2`
- `0 <= lefti <= righti < s.length`

comment: more solution for bs tag

```cpp
class Solution {
public:
    vector<int> platesBetweenCandles(string s, vector<vector<int>>& queries) {
        vector<int> res;
        int n=s.size();

        vector<int> next(n,INT_MAX);//includeing curIdx, next candle idx
        int lastSeen=INT_MAX;
        for(int i=n-1; i>=0; i--){
            if(s[i]=='|') lastSeen=i;
            next[i]=lastSeen;
        }

        vector<int> prev(n,INT_MIN);//includeing curIdx, prev candle idx
        lastSeen=INT_MIN;
        for(int i=0; i<n; i++){
            if(s[i]=='|') lastSeen=i;
            prev[i]=lastSeen;
        }

        vector<int> prefixSum(n,0);//candles prefix sum
        int sum=0;
        for(int i=0;i<n;i++){
            sum+= (s[i]=='|');
            prefixSum[i]=sum;
        }
        function getPrefixSum = [&](int l, int r){return prefixSum[r] - (l==0?0:prefixSum[l-1]);};

        for(auto q: queries){
            int l=q[0], r=q[1];
            l=next[l], r=prev[r];
            if(l>=r) {res.push_back(0); continue;}
            int cnt= (r-l+1) - getPrefixSum(l,r);
            res.push_back(cnt);
        }
        return res;
    }
};

/*
s.len:  3~1e5
queries.len: 1~1e5
================================
s - can't sort, queries- maybe
if not sort queries:
    for each q, 1e5
        need to O(logN) or O(1) to find res?
            - O(logn) - doable?
                bs to find first and last candle, then use prefixsum(l,r) - cnt of candles
            - O(1)
if sort queries:
    sort by b in q[a,b]
*/
```