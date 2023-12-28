# Number of Pairs of Strings With Concatenation Equal to Target

#: 2023
Difficult: Medium
Tags: Hash, string
link: https://leetcode.com/problems/number-of-pairs-of-strings-with-concatenation-equal-to-target/
Priority: Pass
Created time: June 18, 2023 12:10 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an array of **digit** strings `nums` and a **digit** string `target`, return *the number of pairs of indices* `(i, j)` *(where* `i != j`*) such that the **concatenation** of* `nums[i] + nums[j]` *equals* `target`.

**Example 1:**

```
Input: nums = ["777","7","77","77"], target = "7777"
Output: 4
Explanation: Valid pairs are:
- (0, 1): "777" + "7"
- (1, 0): "7" + "777"
- (2, 3): "77" + "77"
- (3, 2): "77" + "77"

```

**Example 2:**

```
Input: nums = ["123","4","12","34"], target = "1234"
Output: 2
Explanation: Valid pairs are:
- (0, 1): "123" + "4"
- (2, 3): "12" + "34"

```

**Example 3:**

```
Input: nums = ["1","1","1"], target = "11"
Output: 6
Explanation: Valid pairs are:
- (0, 1): "1" + "1"
- (1, 0): "1" + "1"
- (0, 2): "1" + "1"
- (2, 0): "1" + "1"
- (1, 2): "1" + "1"
- (2, 1): "1" + "1"

```

**Constraints:**

- `2 <= nums.length <= 100`
- `1 <= nums[i].length <= 100`
- `2 <= target.length <= 100`
- `nums[i]` and `target` consist of digits.
- `nums[i]` and `target` do not have leading zeros.

```cpp
class Solution {
public:
    int numOfPairs(vector<string>& nums, string target) {
        unordered_multiset<string> st(nums.begin(),nums.end());
        int res=0;
        for(auto num: nums){
            if(num.size()>target.size()) continue;
            if(target.substr(0,num.size()) == num){
                if(num == target.substr(num.size())){
                    res+=st.count(target.substr(num.size()))-1;
                }
                else{
                    res+=st.count(target.substr(num.size()));
                }
            }
        }
        return res;
    }
};
```