# Kth Missing Positive Number

#: 1539
Difficult: Easy
Tags: Array, BS_todo
link: https://leetcode.com/problems/kth-missing-positive-number/description/
Priority: High
Status: HardToImplement, More Solution, No Idea At First, toRevisit
Created time: December 3, 2023 6:50 PM
Last edited time: December 3, 2023 7:15 PM
source: facebookFreq

Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.

Return *the* `kth` ***positive** integer that is **missing** from this array.*

**Example 1:**

```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation:The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.

```

**Example 2:**

```
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation:The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.

```

**Constraints:**

- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
- `1 <= k <= 1000`
- `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

**Follow up:**

Could you solve this problem in less than O(n) complexity?

Solution O(N)

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& nums, int k) {
        int x=0;
        int idx=0;
        for(int i=0;i<k;i++){
            for(x+=1; idx<nums.size() && x==nums[idx]; x++, idx++);
        }
        return x;
    }
};
```