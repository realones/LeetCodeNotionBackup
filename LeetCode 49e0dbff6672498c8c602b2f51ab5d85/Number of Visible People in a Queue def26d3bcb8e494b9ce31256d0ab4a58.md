# Number of Visible People in a Queue

#: 1944
Difficult: Hard
Tags: Array, monotonic, stack
link: https://leetcode.com/problems/number-of-visible-people-in-a-queue/description/
Priority: High
Status: No Idea At First
Created time: December 6, 2023 4:57 PM
Last edited time: December 6, 2023 4:58 PM
source: ticktokFreq

There are `n` people standing in a queue, and they numbered from `0` to `n - 1` in **left to right** order. You are given an array `heights` of **distinct** integers where `heights[i]` represents the height of the `ith` person.

A person can **see** another person to their right in the queue if everybody in between is **shorter** than both of them. More formally, the `ith` person can see the `jth` person if `i < j` and `min(heights[i], heights[j]) > max(heights[i+1], heights[i+2], ..., heights[j-1])`.

Return *an array* `answer` *of length* `n` *where* `answer[i]` *is the **number of people** the* `ith` *person can **see** to their right in the queue*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/29/queue-plane.jpg](https://assets.leetcode.com/uploads/2021/05/29/queue-plane.jpg)

```
Input: heights = [10,6,8,5,11,9]
Output: [3,1,2,1,1,0]
Explanation:
Person 0 can see person 1, 2, and 4.
Person 1 can see person 2.
Person 2 can see person 3 and 4.
Person 3 can see person 4.
Person 4 can see person 5.
Person 5 can see no one since nobody is to the right of them.

```

**Example 2:**

```
Input: heights = [5,1,2,3,10]
Output: [4,1,1,1,0]

```

**Constraints:**

- `n == heights.length`
- `1 <= n <= 105`
- `1 <= heights[i] <= 105`
- All the values of `heights` are **unique**.

```cpp
//cp from lee215 to save time
class Solution {
public:
    vector<int> canSeePersonsCount(vector<int>& A) {
        int n = A.size();
        vector<int> res(n), stack;
        for (int i = 0; i < n; ++i) {
            while (!stack.empty() && A[stack.back()] <= A[i])
                res[stack.back()]++, stack.pop_back();
            if (!stack.empty())
                res[stack.back()]++;
            stack.push_back(i);
        }
        return res;
    }
};

/*
You can see the right next greater element.
Your left next greater element can see you.
*/
```