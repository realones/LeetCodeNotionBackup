# Minimum Number of Operations to Make Array Continuous

#: 2009
Difficult: Hard
Tags: Array, BS_lower_upper_bound, Sliding Window
link: https://leetcode.com/problems/minimum-number-of-operations-to-make-array-continuous/
Priority: High
Status: No Idea At First
Created time: October 11, 2023 12:31 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an integer array `nums`. In one operation, you can replace **any** element in `nums` with **any** integer.

`nums` is considered **continuous** if both of the following conditions are fulfilled:

- All elements in `nums` are **unique**.
- The difference between the **maximum** element and the **minimum** element in `nums` equals `nums.length - 1`.

For example, `nums = [4, 2, 5, 3]` is **continuous**, but `nums = [1, 2, 3, 5, 6]` is **not continuous**.

Return *the **minimum** number of operations to make* `nums` ***continuous***.

**Example 1:**

```
Input: nums = [4,2,5,3]
Output: 0
Explanation: nums is already continuous.

```

**Example 2:**

```
Input: nums = [1,2,3,5,6]
Output: 1
Explanation: One possible solution is to change the last element to 4.
The resulting array is [1,2,3,5,4], which is continuous.

```

**Example 3:**

```
Input: nums = [1,10,100,1000]
Output: 3
Explanation: One possible solution is to:
- Change the second element to 2.
- Change the third element to 3.
- Change the fourth element to 4.
The resulting array is [1,2,3,4], which is continuous.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`

Comment: “fix left and find right”, not able to figure out how to start from it

Solution 1:

```cpp
//Editorial - BS
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        //sort dedup
        set<int> st(nums.begin(), nums.end());
        nums = vector<int>(st.begin(), st.end());
        int res=n;
        for(int i=0;i<nums.size();i++){
            int l=nums[i], r=nums[i]+n-1;
            int j=upper_bound(nums.begin()+i, nums.end(),r) - nums.begin();
            res=min(res, n- (j-i));
        }
        return res;
    }
};
```

Solution 2:

```cpp
//Editorial - Sliding Window
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        //sort dedup
        set<int> st(nums.begin(), nums.end());
        nums = vector<int>(st.begin(), st.end());
        int res=n;
        for(int i=0,j=0;i<nums.size();i++){
            int l=nums[i];
            while(j<nums.size() && nums[j]-nums[i]+1<=n) j++;
            res=min(res, n- (j-i));
        }
        return res;
    }
};
```