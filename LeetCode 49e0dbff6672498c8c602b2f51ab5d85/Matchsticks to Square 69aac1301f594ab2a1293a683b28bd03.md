# Matchsticks to Square

#: 473
Difficult: Medium
Tags: Array, BackTracking, Bit Manipulation, BitMask, DP
link: https://leetcode.com/problems/matchsticks-to-square/description/
Priority: High
Status: More Solution, No Idea At First, toSummarize
Created time: December 14, 2023 7:47 PM
Last edited time: December 14, 2023 10:24 PM
source: microsoftFreq

Topics

Companies

Hint

You are given an integer array `matchsticks` where `matchsticks[i]` is the length of the `ith` matchstick. You want to use **all the matchsticks** to make one square. You **should not break** any stick, but you can link them up, and each matchstick must be used **exactly one time**.

Return `true` if you can make this square and `false` otherwise.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg](https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg)

```
Input: matchsticks = [1,1,2,2,2]
Output: true
Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.

```

**Example 2:**

```
Input: matchsticks = [3,3,3,3,4]
Output: false
Explanation: You cannot find a way to form a square with all the matchsticks.

```

**Constraints:**

- `1 <= matchsticks.length <= 15`
- `1 <= matchsticks[i] <= 108`

toSummarize: the three optimizations of the backtracking

```cpp
//backtracking - NP - O(4^n)
//ans: https://leetcode.com/problems/matchsticks-to-square/solutions/95744/cpp-6ms-solution-with-dfs/
class Solution {
public:
    bool makesquare(vector<int>& nums) {
        int total = accumulate(nums.begin(), nums.end(), 0);
        if(total%4!=0) return false;
        int len=total/4;
        for(auto x: nums){
            if(x>len) return false;
        }
        sort(nums.begin(), nums.end(), greater<>());//sort to cut branch earlier

        vector<int> groups(4,0);
        return backtracking(nums,groups,len,0);
    }

    bool backtracking(vector<int>& nums, vector<int>& groups, int& len, int idx){
        if(idx==nums.size()){
            return (groups[0]==len && groups[1]==len && groups[2]==len && groups[3]==len);
        }

        for(int i=0; i<4; i++){
            //cut branch if required
            if(groups[i]+nums[idx]>len) continue;
            //no need to check same group[x] val
            int k=0;
            for(;k<i;k++){
                if(groups[k]==groups[i]) break;
            }
            if(k<i) continue;
            //backtracking main body
            groups[i]+=nums[idx];
            if(backtracking(nums,groups,len,idx+1)) return true;
            groups[i]-=nums[idx];
        }
        return false;
    }
};
```