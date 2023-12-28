# Maximum Element After Decreasing and Rearranging

#: 1846
Difficult: Medium
Tags: Array, Greedy, Sort
link: https://leetcode.com/problems/maximum-element-after-decreasing-and-rearranging/
Priority: Pass
Status: checkBetterSolution
Created time: November 15, 2023 12:08 AM
Last edited time: November 15, 2023 12:09 AM
source: leetcode_daily

You are given an array of positive integers `arr`. Perform some operations (possibly none) on `arr` so that it satisfies these conditions:

- The value of the **first** element in `arr` must be `1`.
- The absolute difference between any 2 adjacent elements must be **less than or equal to** `1`. In other words, `abs(arr[i] - arr[i - 1]) <= 1` for each `i` where `1 <= i < arr.length` (**0-indexed**). `abs(x)` is the absolute value of `x`.

There are 2 types of operations that you can perform any number of times:

- **Decrease** the value of any element of `arr` to a **smaller positive integer**.
- **Rearrange** the elements of `arr` to be in any order.

Return *the **maximum** possible value of an element in* `arr` *after performing the operations to satisfy the conditions*.

**Example 1:**

```
Input: arr = [2,2,1,2,1]
Output: 2
Explanation:
We can satisfy the conditions by rearrangingarr so it becomes[1,2,2,2,1].
The largest element inarr is 2.

```

**Example 2:**

```
Input: arr = [100,1,1000]
Output: 3
Explanation:
One possible way to satisfy the conditions is by doing the following:
1. Rearrangearr so it becomes[1,100,1000].
2. Decrease the value of the second element to 2.
3. Decrease the value of the third element to 3.
Nowarr = [1,2,3], whichsatisfies the conditions.
The largest element inarr is 3.
```

**Example 3:**

```
Input: arr = [1,2,3,4,5]
Output: 5
Explanation: The array already satisfies the conditions, and the largest element is 5.

```

**Constraints:**

- `1 <= arr.length <= 105`
- `1 <= arr[i] <= 109`

comment: only beat 5%, check better solution

```cpp
class Solution {
public:
    int maximumElementAfterDecrementingAndRearranging(vector<int>& nums) {
        nums.push_back(0);
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        for(int i=1; i<n; i++){
            if(nums[i]-nums[i-1]>1){
                nums[i]=nums[i-1]+1;
            }
        }
        return nums.back();
    }
};

/*
len: 1~1e5
val: 1~1e9

sort T:O(nlogn)

*/
```