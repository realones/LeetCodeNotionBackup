# Minimize Maximum of Array

#: 2439
Difficult: Medium
Tags: Array, BS_Val, DP, Greedy, Prefix
link: https://leetcode.com/problems/minimize-maximum-of-array/description/
Priority: High
Status: More Solution, No Idea At First
Created time: October 16, 2023 5:47 PM
Last edited time: October 16, 2023 6:08 PM
source: wisdompeak

You are given a **0-indexed** array `nums` comprising of `n` non-negative integers.

In one operation, you must:

- Choose an integer `i` such that `1 <= i < n` and `nums[i] > 0`.
- Decrease `nums[i]` by 1.
- Increase `nums[i - 1]` by 1.

Return *the **minimum** possible value of the **maximum** integer of* `nums` *after performing **any** number of operations*.

**Example 1:**

```
Input: nums = [3,7,1,6]
Output: 5
Explanation:
One set of optimal operations is as follows:
1. Choose i = 1, and nums becomes [4,6,1,6].
2. Choose i = 3, and nums becomes [4,6,2,5].
3. Choose i = 1, and nums becomes [5,5,2,5].
The maximum integer of nums is 5. It can be shown that the maximum number cannot be less than 5.
Therefore, we return 5.

```

**Example 2:**

```
Input: nums = [10,1]
Output: 10
Explanation:
It is optimal to leave nums as is, and since 10 is the maximum value, we return 10.

```

**Constraints:**

- `n == nums.length`
- `2 <= n <= 1e5`
- `0 <= nums[i] <= 1e9`

Solution BS - wisdompeak:

从右向左传递，从右向左抹平

最终长得这个样：

｜｜

｜｜｜｜｜｜｜

｜｜｜｜｜｜｜｜｜

｜｜｜｜｜｜｜｜｜｜｜｜

BS_val:

left: nums[0], right: max_val

mid check valid

valid(x): XXXXYYYYY

max val (eventrually nums[0]) == x

⇒

all val <= x

如果当前idx可以容纳更多数字

buffer = x-cur

```cpp
class Solution {
public:
    int minimizeArrayValue(vector<int>& nums) {
        int n=nums.size();
        int l=nums[0],r=*max_element(nums.begin(),nums.end());
        function valid = [&](int x)->bool{
            long long buffer=0;
            for(auto num: nums){
                if(num>x){
                    buffer-=num-x;
                    if(buffer<0) return false;
                }
                else{
                    buffer+=x-num;
                }
            }
            return true;
        };
        while(l<r){
            int m=l+(r-l)/2;
            if(!valid(m)) {l=m+1;}
            else {r=m;}
        }
        return l;
    }
};

/*
len: 2~1e5
val always >=0

greedy: always press the largest num?

op seems like press vals grandually to left side

*/
```