# Merge Operations to Turn Array Into a Palindrome

#: 2422
Difficult: Medium
Tags: 2pt, Array, Greedy
link: https://leetcode.com/problems/merge-operations-to-turn-array-into-a-palindrome
Priority: Medium
Status: checkBetterSolution
Created time: May 12, 2023 2:44 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq, wisdompeak, wp-2pt

You are given an array `nums` consisting of **positive** integers.

You can perform the following operation on the array **any** number of times:

- Choose any two **adjacent** elements and **replace** them with their **sum**.
    - For example, if `nums = [1,2,3,1]`, you can apply one operation to make it `[1,5,1]`.

Return *the **minimum** number of operations needed to turn the array into a **palindrome***.

**Example 1:**

```
Input: nums = [4,3,2,1,2,3,1]
Output: 2
Explanation: We can turn the array into a palindrome in 2 operations as follows:
- Apply the operation on the fourth and fifth element of the array, nums becomes equal to [4,3,2,3,3,1].
- Apply the operation on the fifth and sixth element of the array, nums becomes equal to [4,3,2,3,4].
The array [4,3,2,3,4] is a palindrome.
It can be shown that 2 is the minimum number of operations needed.

```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: 3
Explanation: We do the operation 3 times in any position, we obtain the array [10] at the end which is a palindrome.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 106`

Solution:
there should be solution that only using 2pt instead of changing the array

```cpp
using ll=long long;

class Solution {
public:
    int minimumOperations(vector<int>& arr) {
        vector<ll> nums(arr.begin(), arr.end());
        int n=nums.size();// if(n==0) {return 0;}
        int op=0;
        int l=0, r=n-1;
        while(l<r){
            if(nums[l]==nums[r]) l++,r--;
            else if(nums[l]<nums[r]){
                l++;
                nums[l]+=nums[l-1];
                op++;
            }
            else{
                r--;
                nums[r]+=nums[r+1];
                op++;
            }
        }
        return op;
    }
};

/*
op:merge two number
return: min op to make palindrome
================================
len: 1~1e5
val: 1~1e6
================================

*/
```