# [lint] Two Sum - Difference equals to target

#: 610
Difficult: Medium
Tags: 2pt_SameDir_diffSpeed
link: https://www.lintcode.com/problem/two-sum-difference-equals-to-target
Priority: Medium
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 9:59 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers
related: Count Number of Pairs With Absolute Difference K (Count%20Number%20of%20Pairs%20With%20Absolute%20Difference%20K%20bb6cda2bc23640ab8f2027f18bc6aa56.md)

Given an sorted array of integers, find two numbers that their `difference` equals to a target value.
return a list with two number like `[num1, num2]` that the difference of `num1` and `num2` equals to target value, and `num1` is less than `num2`.

Example:
Example 1:

```
Input: nums = [2, 7, 15, 24], target = 5
Output: [2, 7]
Explanation:
(7 - 2 = 5)

```

Example 2:

```
Input: nums = [1, 1], target = 0
Output: [1, 1]
Explanation:
(1 - 1 = 0)

```

**Solutions**:

Solution 2pt

```cpp
class Solution {
public:
    /**
     * @param nums: an array of Integer
     * @param target: an integer
     * @return: [num1, num2] (num1 < num2)
     */
    vector<int> twoSum7(vector<int> &nums, int target) {
        // write your code here
        int n=nums.size(); if(n<2) {return {-1,-1};}
        target=abs(target);
        int l=0,r=1;
        while(r<n){
            int diff=nums[r]-nums[l];
            if(diff==target){return {nums[l],nums[r]};}

            if(diff < target){
                r++;
            }
            else{
                l++;
                if(l==r) r++;
            }
        }
        return {-1,-1};
    }
};
```

Solution hash:

```cpp
class Solution {
public:
	/*
	* @param nums: an array of Integer
	* @param target: an integer
	* @return: [index1 + 1, index2 + 1] (index1 < index2)
	*/
	vector<int> twoSum7(vector<int> &nums, int target) {
		if (nums.size()<2){ return vector<int>(); }
		target = abs(target);
		unordered_map<int,int> mp;
		for (int i = 0; i < nums.size(); i++){
			if (mp.count(nums[i] - target) || mp.count(nums[i] + target)){//if success
				//assume at most 1 result
				int left;
				if(mp.count(nums[i] - target))  left = mp[nums[i] - target];
				else left = mp[nums[i] + target];
				int right = i;

				vector<int> res;
				res.push_back(left+1);
				res.push_back(right+1);
				sort(res.begin(),res.end());
				return res;
			}

			mp[nums[i]]=i;
		}

		//assume (-1,-1)
		return vector<int>(2, -1);
	}
};
```