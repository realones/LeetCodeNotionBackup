# Removing Minimum and Maximum From Array

#: 2091
Difficult: Medium
Tags: Array, Greedy
link: https://leetcode.com/problems/removing-minimum-and-maximum-from-array/description/
Priority: Low
Created time: December 24, 2023 5:15 PM
Last edited time: December 24, 2023 5:15 PM
source: googleFreq

You are given a **0-indexed** array of **distinct** integers `nums`.

There is an element in `nums` that has the **lowest** value and an element that has the **highest** value. We call them the **minimum** and **maximum** respectively. Your goal is to remove **both** these elements from the array.

A **deletion** is defined as either removing an element from the **front** of the array or removing an element from the **back** of the array.

Return *the **minimum** number of deletions it would take to remove **both** the minimum and maximum element from the array.*

**Example 1:**

```
Input: nums = [2,10,7,5,4,1,8,6]
Output: 5
Explanation:
The minimum element in the array is nums[5], which is 1.
The maximum element in the array is nums[1], which is 10.
We can remove both the minimum and maximum by removing 2 elements from the front and 3 elements from the back.
This results in 2 + 3 = 5 deletions, which is the minimum number possible.

```

**Example 2:**

```
Input: nums = [0,-4,19,1,8,-2,-3,5]
Output: 3
Explanation:
The minimum element in the array is nums[1], which is -4.
The maximum element in the array is nums[2], which is 19.
We can remove both the minimum and maximum by removing 3 elements from the front.
This results in only 3 deletions, which is the minimum number possible.

```

**Example 3:**

```
Input: nums = [101]
Output: 1
Explanation:
There is only one element in the array, which makes it both the minimum and maximum element.
We can remove it with 1 deletion.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `105 <= nums[i] <= 105`
- The integers in `nums` are **distinct**.

```cpp
class Solution {
public:
    int minimumDeletions(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        if(n<=2) return n;
        int a = min_element(nums.begin(), nums.end())-nums.begin();
        int b = max_element(nums.begin(), nums.end())-nums.begin();
        if(a>b) swap(a,b);
        //talk about different cases
        return min({
            b+1,
            n-a,
            a+1 + n-b
        });
    }
};

/*
....a....b....

case 1: delete from left to b
case 2: delete from right to a
case 3: delete from left to a, delete from right to b
*/
```