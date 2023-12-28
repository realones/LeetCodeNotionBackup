# Minimize Deviation in Array

#: 1675
Difficult: Hard
Tags: Array, Greedy, OrderSet, PQ
link: https://leetcode.com/problems/minimize-deviation-in-array/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: June 25, 2023 4:27 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

- If the element is **even**, **divide** it by `2`.
    - For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
- If the element is **odd**, **multiply** it by `2`.
    - For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The **deviation** of the array is the **maximum difference** between any two elements in the array.

Return *the **minimum deviation** the array can have after performing some number of operations.*

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.

```

**Example 2:**

```
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.

```

**Example 3:**

```
Input: nums = [2,10,8]
Output: 3

```

**Constraints:**

- `n == nums.length`
- `2 <= n <= 5 * 104`
- `1 <= nums[i] <= 109`

Solution OrderSet:

完全想不到怎么做，似乎涉及到一点数学证明，参考了huahua的解法
lee215似乎有先全除到odd的解法，以后学习

```cpp
class Solution {
public:
    int minimumDeviation(vector<int>& nums) {//val: 1~1e9
        set<int> st;
        for(auto x: nums){
            st.insert(x&1? x*2: x);
        }
        int res=*st.rbegin() - *st.begin();
        while(*st.rbegin()%2==0){
            st.insert(*st.rbegin()/2);
            st.erase(*st.rbegin());
            res=min(res,*st.rbegin()-*st.begin());
        }
        return res;
    }
};

/*
from huahua:
If we double an odd number it becomes an even number, then we can only divide it by two which gives us back the original number. So we can pre-double all the odd numbers and only do division in the following process.

We push all numbers including pre-doubled odd ones onto a priority queue, and track the difference between the largest and smallest number.

Each time, we pop the largest number out and divide it by two then put it back to the priority queue, until the largest number becomes odd. We can not discard it and divide any other smaller numbers by two will only increase the max difference, so we can stop here.

ex1: [3, 5, 8] => [6, 8, 10] (pre-double) => [5, 6, 8] => [4, 5, 6] => [3, 4, 5] max diff is 5 – 3 = 2
ex2: [4,1,5,20,3] => [2, 4, 6, 10, 20] (pre-double) => [2, 4, 6, 10] => [2, 4, 5, 6] => [2,3,4,5] max diff = 5-2 = 3

Time complexity: O(n*logm*logn)
Space complexity: O(n)
*/
```

Solution PQ:

same thought with above one, but manually track the lowest val

```cpp
class Solution {
public:
    int minimumDeviation(vector<int>& nums) {
        priority_queue<int> pq;
        int lo=INT_MAX;
        for(int x: nums){
            x=x&1? x*2: x;
            pq.push(x);
            lo=min(lo,x);
        }
        int res=pq.top()-lo;
        while(pq.top()%2==0){
            int x=pq.top(); pq.pop();
            pq.push(x/2);
            lo=min(lo,x/2);
            res=min(res,pq.top()-lo);
        }
        return res;
    }
};
```