# Maximum Distance in Arrays

#: 624
Difficult: Medium
Tags: Array, Greedy, Prefix
link: https://leetcode.com/problems/maximum-distance-in-arrays/
Priority: Medium
Status: More Solution, toSummarize
Created time: May 25, 2023 12:57 AM
Last edited time: November 21, 2023 5:07 PM
source: LC_Premium_Algo_100

You are given `m` `arrays`, where each array is sorted in **ascending order**.

You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers `a` and `b` to be their absolute difference `|a - b|`.

Return *the maximum distance*.

**Example 1:**

```
Input: arrays = [[1,2,3],[4,5],[1,2,3]]
Output: 4
Explanation: One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.

```

**Example 2:**

```
Input: arrays = [[1],[1]]
Output: 0

```

**Constraints:**

- `m == arrays.length`
- `2 <= m <= 105`
- `1 <= arrays[i].length <= 500`
- `104 <= arrays[i][j] <= 104`
- `arrays[i]` is sorted in **ascending order**.
- There will be at most `105` integers in all the arrays.

toSummarize:

solution prefix is to think for i, how to find best j

solution 1pss is to think for cur, what is the lastBest j

to summarize the same and diff, what kind of questions it can solve

Solution 1pass:

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int n=arrays.size();//n: 2~1e5
        int minV=arrays[0].front(), maxV=arrays[0].back();
        int res=0;
        for(int i=1; i<n; i++){
            int curMin=arrays[i].front();
            int curMax=arrays[i].back();
            res=max({res, maxV-curMin, curMax-minV});
            minV=min(minV, curMin);
            maxV=max(maxV, curMax);
        }
        return res;
    }
};
```

Solution prefix:

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int n=arrays.size();
        //pre_process maxV[i]: max element for all except arrays[i]
        vector<int> maxV(n,INT_MIN);
        vector<int> prefix(n,INT_MIN);
        vector<int> suffix(n,INT_MIN);
        for(int i=0; i<n; i++){
            prefix[i]=max(arrays[i].back(), i-1>=0?prefix[i-1]:INT_MIN);
        }
        for(int i=n-1; i>=0; i--){
            suffix[i]=max(arrays[i].back(), i+1<n? suffix[i+1]:INT_MIN);
        }
        for(int i=0; i<n; i++){
            maxV[i]=max(
                    (i-1>=0?prefix[i-1]:INT_MIN),
                    (i+1<n? suffix[i+1]:INT_MIN)
                );
        }

        //process
        int res=0;
        for(int i=0; i<n; i++){
            int minV=arrays[i].front();
            int tmp=maxV[i]-minV;
            res=max(res,tmp);
        }
        return res;
    }
};
```