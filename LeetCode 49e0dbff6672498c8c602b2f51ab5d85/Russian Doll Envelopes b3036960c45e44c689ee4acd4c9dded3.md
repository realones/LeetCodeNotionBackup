# Russian Doll Envelopes

#: 354
Difficult: Hard
Tags: Array, DP, GoForth, UsePreResults
link: https://leetcode.com/problems/russian-doll-envelopes/
Priority: Pass
Created time: August 18, 2022 12:25 PM
Last edited time: October 22, 2023 12:11 AM
practicedTimes: 1
source: jiuzhang, 算初DP

You are given a 2D array of integers `envelopes` where `envelopes[i] = [wi, hi]` represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return *the maximum number of envelopes you can Russian doll (i.e., put one inside the other)*.

**Note:** You cannot rotate an envelope.

**Example 1:**

```
Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
Output: 3
Explanation: The maximum number of envelopes you can Russian doll is3 ([2,3] => [5,4] => [6,7]).

```

**Example 2:**

```
Input: envelopes = [[1,1],[1,1],[1,1]]
Output: 1

```

**Constraints:**

- `1 <= envelopes.length <= 105`
- `envelopes[i].length == 2`
- `1 <= wi, hi <= 105`

```cpp
//Sort the array. Ascend on width and descend on height if width are same.
//Find the longest increasing subsequence based on height.
bool cmp(const pair<int,int>&x, const pair<int, int>&y) {
  return x.first != y.first ? x.first < y.first : x.second > y.second;
}

class Solution {
public:
    /**
     * @param envelopes a number of envelopes with widths and heights
     * @return the maximum number of envelopes
     */
    int maxEnvelopes(vector<pair<int, int>>& envelopes) {
        // Write your code here
        int n = envelopes.size();
        if (n == 0) {
            return 0;
        }
        int res=1;
        sort(envelopes.begin(), envelopes.end(), cmp);
        vector<int> dp(n,1);
        for(int k=1;k<n;k++){
            for(int i=0;i<k;i++){
                if(envelopes[i].second>=envelopes[k].second) continue;//when pre higher
                dp[k]=max(dp[k],dp[i]+1);
                res=max(res,dp[k]);
            }
        }
        return res;
```