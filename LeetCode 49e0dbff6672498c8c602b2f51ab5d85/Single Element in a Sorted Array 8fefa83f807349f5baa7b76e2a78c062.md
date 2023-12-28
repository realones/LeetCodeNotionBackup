# Single Element in a Sorted Array

#: 540
Difficult: Medium
Tags: Array, BS_Idx
link: https://leetcode.com/problems/single-element-in-a-sorted-array/description/
Priority: High
Status: Dig More, HardToImplement, checkBetterSolution, toSummarize
Created time: June 27, 2023 11:35 PM
Last edited time: December 24, 2023 4:18 PM
source: HuaHua, googleFreq

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return *the single element that appears only once*.

Your solution must run in `O(log n)` time and `O(1)` space.

**Example 1:**

```
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2

```

**Example 2:**

```
Input: nums = [3,3,7,7,10,11,11]
Output: 10

```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 105`

comment: there should be better solution, i only beat 5%

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        std::cout<< "nums" <<" -- "; for (const auto& elem : nums) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        int n=nums.size();
        int l=0, r=n-1;
        while(l<r){
            std::cout << "l,r" << " -- " << l<<","<<r << std::endl;
            int m=l+(r-l)/2;
            //case1
            if((m-l)%2==0){
                if(nums[m]!=nums[m-1] && nums[m]!=nums[m+1]) return nums[m];
                else if(nums[m]==nums[m-1] && nums[m]!=nums[m+1]) r=m;
                else if(nums[m]==nums[m+1] && nums[m]!=nums[m-1]) l=m;
            }
            //case2
            else{
                if(nums[m]==nums[m-1] && nums[m]!=nums[m+1]) l=m+1;
                else if(nums[m]==nums[m+1] && nums[m]!=nums[m-1]) r=m-1;
            }
        }
        return nums[l];
    }
};

/*
brute force: check from left to right if there is single elem - O(n)

bs:

AATBB, !=l, !=r
TAABB, ==l, !=r
AABBT, ==r, !=l

XXAATXX, ==l, !=r
  TAA, ==r, !=l

*/
```