# Collecting Chocolates

#: 2735
Difficult: Medium
Tags: Array, Greedy
link: https://leetcode.com/problems/collecting-chocolates/
Priority: High
Status: No Idea At First
Created time: June 10, 2023 9:32 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` of size `n` representing the cost of collecting different chocolates. Each chocolate is of a different type, and originally, the chocolate at `ith` index is of `ith` type.

In one operation, you can do the following with an incurred **cost** of `x`:

- Simultaneously change the chocolate of `ith` type to (`i + 1)th` type for all indexes `i` where `0 <= i < n - 1`. When `i == n - 1`, that chocolate will be changed to type of the chocolate at index `0`.

Return *the minimum cost to collect chocolates of all types, given that you can perform as many operations as you would like.*

**Example 1:**

```
Input: nums = [20,1,15], x = 5
Output: 13
Explanation: Initially, the chocolate types are [0,1,2]. We will buy the 1st type of chocolate at a cost of 1.
Now, we will perform the operation at a cost of 5, and the types of chocolates will become [2,0,1]. We will buy the 0thtype of chocolate at a cost of 1.
Now, we will again perform the operation at a cost of 5, and the chocolate types will become [1,2,0]. We will buy the 2ndtype of chocolate at a cost of 1.
Thus, the total cost will become (1 + 5 + 1 + 5 + 1) = 13. We can prove that this is optimal.

```

**Example 2:**

```
Input: nums = [1,2,3], x = 4
Output: 6
Explanation: We will collect all three types of chocolates at their own price without performing any operations. Therefore, the total cost is 1 + 2 + 3 = 6.

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 109`
- `1 <= x <= 109`

**Solution**:

**Preword**

I feel,
the description and the explanation in example 1,
are totaly opposite...

**Intuition**

If we do rotate operation k times,
we can use the min(A[i-k], .. , A[i - 1], A[i]) as the cost for type i.

**Explanation**

Initilize result array res.
res[k] means the result for k times operation,
and we initilize res[k] = k * x for rotation cost.

To collect type i
we calculate cur = min(A[i-k], .. , A[i - 1], A[i]),
and increment res[k] += cur,
so we calculate the minimum cost to get type i with k operation.

Finally we return the min(res) as final result.

**Complexity**

Time O(n^2)
Space O(n)

Solution 1:

```cpp
class Solution {
public:
    long long minCost(vector<int>& A, int x) {
        int n = A.size();
        vector<int> minA(A.begin(), A.end());
        long long res=LLONG_MAX;
        for (int k = 0; k < n; k++) {//rotate k times
            long long tmpRes=1L*k*x;
            for(int i=0;i<n;i++){//idx
                minA[i]=min(minA[i],A[(i-k+n)%n]);
                tmpRes+=minA[i];
            }
            res=min(res,tmpRes);
        }
        return res;
    }
};
```

Solution 2 - not understand:

```cpp
class Solution {
public:
    long long minCost(vector<int>& A, int x) {
        int n = A.size();
        vector<long long> res(n);
        for (int i = 0; i < n; i++) {
            res[i] += 1L * i * x;
            int cur = A[i];
            for (int k = 0; k < n; k++) {
                cur = min(cur, A[(i - k + n) % n]);
                res[k] += cur;
            }
        }
        return *std::min_element(res.begin(), res.end());
    }
};
```