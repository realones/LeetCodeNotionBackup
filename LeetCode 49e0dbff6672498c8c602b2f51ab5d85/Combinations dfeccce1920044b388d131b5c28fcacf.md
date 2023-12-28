# Combinations

#: 77
Difficult: Medium
Tags: BackTracking
link: https://leetcode.com/problems/combinations/
Priority: Low
Created time: June 5, 2023 1:04 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150, leetcode_daily

Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers chosen from the range* `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.

```

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`

Solution: combination, no need visited array

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> tmp;
    int n;
    
    vector<vector<int>> combine(int n, int k) {
        this->n=n;
        backtracking(1,k);
        return res;
    }
    
    void backtracking(int start, int k){
        if(k==0) {
            res.push_back(tmp);
            return;
        }
        for(int i=start; i<=n; i++){
            tmp.push_back(i);
            backtracking(i+1,k-1);
            tmp.pop_back();
        }
    }
};
```