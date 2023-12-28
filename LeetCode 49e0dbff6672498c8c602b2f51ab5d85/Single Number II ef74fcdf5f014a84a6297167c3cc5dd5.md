# Single Number II

#: 137
Difficult: Medium
Tags: Bit Manipulation
link: https://leetcode.com/problems/single-number-ii/
Priority: Low
Status: No Idea At First
Created time: June 6, 2023 11:15 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given an integer array `nums` where every element appears **three times** except for one, which appears **exactly once**. *Find the single element and return it*.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

```
Input: nums = [2,2,3,2]
Output: 3

```

**Example 2:**

```
Input: nums = [0,1,0,1,0,1,99]
Output: 99

```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `231 <= nums[i] <= 231 - 1`
- Each element in `nums` appears exactly **three times** except for one element which appears **once**.

```cpp
class Solution {
public:
    int singleNumber(vector<int>& A) {
		//simulate: 
		/*
		10110001 appear for three times: 
			30330003
		put all numbers' bits in allbits
		then allbits % 3 = result
		*/
        vector<int> allbits(32,0);
        int res=0;
        for(auto a:A){
            for(int i=0;i<32;i++){
                allbits[i]+=(a>>i)&1;
            }
        }
        for(int i=0;i<32;i++){
            res+=allbits[i]%3<<i;
        }        
        return res;
    }
};
```