# Number of Ways to Reorder Array to Get Same BST

#: 1569
Difficult: Hard
Tags: Array, DP, Divide and Conquer, Math, Tree, Union Find
link: https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/
Priority: Medium
Status: Dig More, out of scope, toSummarize
Created time: June 15, 2023 9:02 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an array `nums` that represents a permutation of integers from `1` to `n`. We are going to construct a binary search tree (BST) by inserting the elements of `nums` in order into an initially empty BST. Find the number of different ways to reorder `nums` so that the constructed BST is identical to that formed from the original array `nums`.

- For example, given `nums = [2,1,3]`, we will have 2 as the root, 1 as a left child, and 3 as a right child. The array `[2,3,1]` also yields the same BST but `[3,2,1]` yields a different BST.

Return *the number of ways to reorder* `nums` *such that the BST formed is identical to the original BST formed from* `nums`.

Since the answer may be very large, **return it modulo** `109 + 7`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/12/bb.png](https://assets.leetcode.com/uploads/2020/08/12/bb.png)

```
Input: nums = [2,1,3]
Output: 1
Explanation: We can reorder nums to be [2,3,1] which will yield the same BST. There are no other ways to reorder nums which will yield the same BST.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/08/12/ex1.png](https://assets.leetcode.com/uploads/2020/08/12/ex1.png)

```
Input: nums = [3,4,5,1,2]
Output: 5
Explanation: The following 5 arrays will yield the same BST:
[3,1,2,4,5]
[3,1,4,2,5]
[3,1,4,5,2]
[3,4,1,2,5]
[3,4,1,5,2]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/08/12/ex4.png](https://assets.leetcode.com/uploads/2020/08/12/ex4.png)

```
Input: nums = [1,2,3]
Output: 0
Explanation: There are no other orderings of nums that will yield the same BST.

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= nums.length`
- All integers in `nums` are **distinct**.

```cpp
class Solution {
public:
    int numOfWays(vector<int>& nums) {
        int m = nums.size();
        // Table of Pascal's triangle - for the weave purpose
        table.resize(m + 1);
        for(int i = 0; i < m + 1; ++i) {
            table[i] = vector<long long>(i + 1, 1);
            for(int j = 1; j < i; ++j) {
                table[i][j] = (table[i - 1][j - 1] + table[i - 1][j]) % mod;
            }
        }
        
        return (dfs(nums) - 1) % mod;
    }
    
private:
    vector<vector<long long>> table;
    long long mod = 1e9 + 7;
    
    long long dfs(vector<int> &nums){
        int m = nums.size();
        if(m == 0) { return 1; }
        if(m == 1) { return 1; }

        vector<int> leftNodes, rightNodes;
        for(int i = 1; i < m; ++i){
            if (nums[i] < nums[0]) {
                leftNodes.push_back(nums[i]);
            } else {
                rightNodes.push_back(nums[i]);
            }
        }
		
        long long leftWays = dfs(leftNodes) % mod;
        long long rightWays = dfs(rightNodes) % mod;
		//weave - out of scope
        return (((leftWays * rightWays) % mod) * table[m - 1][leftNodes.size()]) % mod;
    }
};
```