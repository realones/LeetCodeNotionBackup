# Maximum Length of Repeated Subarray

#: 718
Difficult: Medium
Tags: DP, StringMatchToMatrix
link: https://leetcode.com/problems/maximum-length-of-repeated-subarray/
Priority: Pass
Status: dup
Created time: September 15, 2022 8:22 PM
Last edited time: October 29, 2023 10:49 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算高DP下

Given two integer arrays `nums1` and `nums2`, return *the maximum length of a subarray that appears in **both** arrays*.

**Example 1:**

```
Input: nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
Output: 3
Explanation: The repeated subarray with maximum length is [3,2,1].

```

**Example 2:**

```
Input: nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
Output: 5

```

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 100`

==longest common substring

```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int m=nums1.size(), n=nums2.size();
        vector<vector<int>> f(m+1,vector<int>(n+1,0));
        int res=0;
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(nums1[i-1]==nums2[j-1]){
                    f[i][j]=f[i-1][j-1]+1;
                }
                else{
                    f[i][j]=0;
                }
                res=max(res,f[i][j]);
            }
        }
        return res;
    }
};
```