# Sum of Total Strength of Wizards

#: 2281
Difficult: Hard
Tags: Array, Math, Prefix, monotonic, stack
link: https://leetcode.com/problems/sum-of-total-strength-of-wizards/description/
Priority: High
Status: Dig More, No Idea At First, toSummarize
Created time: October 3, 2023 8:51 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

As the ruler of a kingdom, you have an army of wizards at your command.

You are given a **0-indexed** integer array `strength`, where `strength[i]` denotes the strength of the `ith` wizard. For a **contiguous** group of wizards (i.e. the wizards' strengths form a **subarray** of `strength`), the **total strength** is defined as the **product** of the following two values:

- The strength of the **weakest** wizard in the group.
- The **total** of all the individual strengths of the wizards in the group.

Return *the **sum** of the total strengths of **all** contiguous groups of wizards*. Since the answer may be very large, return it **modulo** `109 + 7`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

```
Input: strength = [1,3,1,2]
Output: 44
Explanation: The following are all the contiguous groups of wizards:
- [1] from [1,3,1,2] has a total strength of min([1]) * sum([1]) = 1 * 1 = 1
- [3] from [1,3,1,2] has a total strength of min([3]) * sum([3]) = 3 * 3 = 9
- [1] from [1,3,1,2] has a total strength of min([1]) * sum([1]) = 1 * 1 = 1
- [2] from [1,3,1,2] has a total strength of min([2]) * sum([2]) = 2 * 2 = 4
- [1,3] from [1,3,1,2] has a total strength of min([1,3]) * sum([1,3]) = 1 * 4 = 4
- [3,1] from [1,3,1,2] has a total strength of min([3,1]) * sum([3,1]) = 1 * 4 = 4
- [1,2] from [1,3,1,2] has a total strength of min([1,2]) * sum([1,2]) = 1 * 3 = 3
- [1,3,1] from [1,3,1,2] has a total strength of min([1,3,1]) * sum([1,3,1]) = 1 * 5 = 5
- [3,1,2] from [1,3,1,2] has a total strength of min([3,1,2]) * sum([3,1,2]) = 1 * 6 = 6
- [1,3,1,2] from [1,3,1,2] has a total strength of min([1,3,1,2]) * sum([1,3,1,2]) = 1 * 7 = 7
The sum of all the total strengths is 1 + 9 + 1 + 4 + 4 + 4 + 3 + 5 + 6 + 7 = 44.

```

**Example 2:**

```
Input: strength = [5,4,6]
Output: 213
Explanation: The following are all the contiguous groups of wizards:
- [5] from [5,4,6] has a total strength of min([5]) * sum([5]) = 5 * 5 = 25
- [4] from [5,4,6] has a total strength of min([4]) * sum([4]) = 4 * 4 = 16
- [6] from [5,4,6] has a total strength of min([6]) * sum([6]) = 6 * 6 = 36
- [5,4] from [5,4,6] has a total strength of min([5,4]) * sum([5,4]) = 4 * 9 = 36
- [4,6] from [5,4,6] has a total strength of min([4,6]) * sum([4,6]) = 4 * 10 = 40
- [5,4,6] from [5,4,6] has a total strength of min([5,4,6]) * sum([5,4,6]) = 4 * 15 = 60
The sum of all the total strengths is 25 + 16 + 36 + 36 + 40 + 60 = 213.

```

**Constraints:**

- `1 <= strength.length <= 105`
- `1 <= strength[i] <= 109`

todo: in the end of this article

Solution: mono stk

```cpp
using ll = long long;
ll M = 1e9+7;

class Solution {
public:
    int totalStrength(vector<int>& nums) 
    {
        nums.insert(nums.begin(), 0);
        nums.push_back(0);
        stack<int> stk({0});
        
        //============return sum of subarr sum with left: l~p, right: r~p
        vector<ll>prefixSum(nums.size(), 0);
        for (int i=1; i<prefixSum.size(); i++)
            prefixSum[i] = (prefixSum[i-1]+(ll)nums[i]) % M;
        function getPrefixSum = [&](int l, int r)->ll{return prefixSum[r]-prefixSum[l-1];};

        vector<ll>prefixSum2(nums.size(), 0);
        for (int i=1; i<prefixSum2.size(); i++)
            prefixSum2[i] = (prefixSum2[i-1]+(ll)nums[i]*i) % M;
        function getPrefixSum2 = [&](int l, int r)->ll{return prefixSum2[r]-prefixSum2[l-1];};
        
        function f = [&](int l, int p, int r)->ll{
            ll len_l_p=p-l+1, len_p_r=r-p+1;
            ll first = ( getPrefixSum2(l,p-1) - getPrefixSum(l,p-1) * (l-1) %M + M) % M;
            first = first * len_p_r % M;
            ll second = (getPrefixSum(p+1,r) * (r+1) - getPrefixSum2(p+1,r) + M ) % M;
            second = second * len_l_p % M;
            ll mid = (ll)nums[p] * len_l_p * len_p_r % M;
            
            return (first + second + mid) * nums[p] % M;  
        };
        //============end
        
        ll res = 0;
        for (int i=1; i<nums.size(); i++)
        {
            while (nums[stk.top()]>nums[i])
            {
                int p=stk.top(); stk.pop();//pivot
                ll l = stk.top()+1, r = i-1;
                res+=f(l,p,r);
                res%=M;
            }
            stk.push(i);            
        }
        return (res+M)%M;
    }
};
```

todo:

from lee215

# **More Good Stack Problems**

Here are some problems that impressed me.

Good luck and have fun.

- [Sum of Total Strength of Wizards](https://leetcode.com/problems/sum-of-total-strength-of-wizards/discuss/2061985/python-solution-on/1405190)
- [Car Fleet II](https://leetcode.com/problems/car-fleet-ii/discuss/1085987/javacpython-on-stack-solution/)
- [Find the Most Competitive Subsequence](https://leetcode.com/problems/find-the-most-competitive-subsequence/discuss/952786/javacpython-one-pass-stack-solution/776191)
- [Minimum Number of Removals to Make Mountain Array](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/discuss/952136/Python-LIS-O(nlogn))
- [Final Prices With a Special Discount in a Shop](https://leetcode.com/problems/final-prices-with-a-special-discount-in-a-shop/discuss/685390/javacpython-stack-one-pass/809992)
- [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/discuss/597751/JavaC++Python-O(N)-Decreasing-Deque)
- [Minimum Cost Tree From Leaf Values](https://leetcode.com/problems/minimum-cost-tree-from-leaf-values/discuss/339959/One-Pass-O(N)-Time-and-Space)
- [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/discuss/170750/C++JavaPython-Stack-Solution)
- [Online Stock Span](https://leetcode.com/problems/online-stock-span/discuss/168311/C++JavaPython-O(1))
- [Score of Parentheses](https://leetcode.com/problems/score-of-parentheses/discuss/141777/C++JavaPython-O(1)-Space)
- [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/discuss/98270/JavaC++Python-Loop-Twice)
- Next Greater Element I
- Largest Rectangle in Histogram
- Trapping Rain Water

# **More Similar Prefix Problems**

Here are some similar prolem that I used prefix sum.

Upvoted, good luck and have fun.

- [Sum of Total Strength of Wizards](https://leetcode.com/problems/sum-of-total-strength-of-wizards/discuss/2061985/python-solution-on/1405190)
- [Number of Submatrices That Sum to Target](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/discuss/303750/JavaC++Python-Find-the-Subarray-with-Target-Sum)
- [Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/discuss/217985/JavaC++Python-Prefix-Sum)