# Maximum XOR of Two Numbers in an Array

#: 421
Difficult: Medium
Tags: Array, Bit Manipulation, Hash, Trie
link: https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/description/
Priority: High
Status: More Solution, No Idea At First, todo
Created time: November 12, 2023 7:15 PM
Last edited time: November 12, 2023 7:18 PM
source: leetcode_daily
related: Maximum Strong Pair XOR II (Maximum%20Strong%20Pair%20XOR%20II%20c05395bc69534d28816172590340fa26.md)

Given an integer array `nums`, return *the maximum result of* `nums[i] XOR nums[j]`, where `0 <= i <= j < n`.

**Example 1:**

```
Input: nums = [3,10,5,25,2,8]
Output: 28
Explanation: The maximum result is 5 XOR 25 = 28.

```

**Example 2:**

```
Input: nums = [14,70,53,83,49,91,36,80,92,51,66,70]
Output: 127

```

**Constraints:**

- `1 <= nums.length <= 2 * 105`
- `0 <= nums[i] <= 231 - 1`

```cpp
class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        int res=0, mask=0;
        for(int i=30;i>=0;i--){
            mask |= (1 << i);
            unordered_set<int> set;
            for(auto num : nums){
                set.insert(num & mask); // reserve Left bits and ignore Right bits
            }
             /* Use 0 to keep the bit, 1 to find XOR
             * 0 ^ 0 = 0 
             * 0 ^ 1 = 1
             * 1 ^ 0 = 1
             * 1 ^ 1 = 0
             */
            int tmp = res | (1 << i); // in each iteration, there are pair(s) whoes Left bits can XOR to max
            for(auto prefix : set){
                if(set.count(tmp ^ prefix)) {
                    res = tmp;
                    break;
                }
            }
        }
        
        return res;
    }
};
```