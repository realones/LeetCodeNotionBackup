# Two Sum

#: 1
Difficult: Easy
Tags: 2pt_DiffDir, Hash
link: https://leetcode.com/problems/two-sum/
Priority: Pass
Created time: August 6, 2022 11:35 PM
Last edited time: October 13, 2023 1:01 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初TwoPointers

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]

```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]

```

**Constraints:**

- `2 <= nums.length <= 104`
- `109 <= nums[i] <= 109`
- `109 <= target <= 109`
- **Only one valid answer exists.**

**Follow-up:**

Can you come up with an algorithm that is less than

```
O(n2)
```

time complexity?

solution: 2pt

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        vector<int> res;
        
        for (int i = 0; i < nums.size(); i++) {
            if(hash.count(target - nums[i])>0){
                res.push_back(hash[target - nums[i]]);
                res.push_back(i);
            }
            hash[nums[i]]=i;
        }
        return res;
    }
};
```

Solution hash:

```cpp
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            }
            map.put(nums[i], i);
        }
        // In case there is no solution, we'll just return null
        return null;
    }
}
```