# [lint] Backpack IV

#: 562
Difficult: Medium
Tags: DP, dp-backpack
link: https://www.lintcode.com/problem/562/
Priority: Low
Created time: September 16, 2022 11:56 PM
Last edited time: October 12, 2023 2:56 PM
source: jiuzhang, 算高DP下

Description

Give `n` items and an array, `num[i]` indicate the size of `i`th item. An integer `target` denotes the size of backpack. Find the number of ways to fill the backpack.

`Each item may be chosen unlimited number of times`

Example

**Example1**

```
Input: nums = [2,3,6,7] and target = 7
Output: 2
Explanation:
Solution sets are:
[7]
[2, 2, 3]

```

**Example2**

```
Input: nums = [2,3,4,5] and target = 7
Output: 3
Explanation:
Solution sets are:
[2, 5]
[3, 4]
[2, 2, 3]

```

Solution:

- State:
• f[i][S] “前i”个物品，有几种组成S的方法
- Function:
• cur=A[i-1] 是第i个物品下标是i-1
• k 是第i个物品选取的次数
• f[i][S] = f[i-1][S] + f[i-1][S - k**a[i-1]] for all available k
for(int k=0; j-k**curSz≥0; k++){
    dp[i][j]+=dp[i-1][j-k*curSz];
}
- Intialize:
• f[0][0] = 1;
- Answer:
• f[n][S]

```cpp
class Solution {
public:
    /**
     * @param nums: an integer array and all positive numbers, no duplicates
     * @param target: An integer
     * @return: An integer
     */
    int backPackIV(vector<int> &nums, int packSz) {
        // write your code here
        int m=nums.size(), n=packSz;
        vector<vector<int>> f(m+1,vector<int>(n+1,0));
        for(int i=0; i<m+1; i++){f[i][0]=1;}
        for(int i=1;i<m+1;i++){
            int curSz=nums[i-1];
            for(int j=1;j<n+1;j++){
                //take 0,1,...k times
                for(int k=0; j-k*curSz>=0; k++){
                    f[i][j]+=f[i-1][j-k*curSz];
                }
            }
        }
        return f.back().back();
    }
};
```