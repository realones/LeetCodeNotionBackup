# Path Sum IV

#: 666
Difficult: Medium
Tags: Array, DFS, Hash, Tree
link: https://leetcode.com/problems/path-sum-iv/description/
Priority: Medium
Status: HardToImplement
Created time: December 5, 2023 5:06 PM
Last edited time: December 5, 2023 5:07 PM
source: ticktokFreq

If the depth of a tree is smaller than `5`, then this tree can be represented by an array of three-digit integers. For each integer in this array:

- The hundreds digit represents the depth `d` of this node where `1 <= d <= 4`.
- The tens digit represents the position `p` of this node in the level it belongs to where `1 <= p <= 8`. The position is the same as that in a full binary tree.
- The units digit represents the value `v` of this node where `0 <= v <= 9`.

Given an array of **ascending** three-digit integers `nums` representing a binary tree with a depth smaller than `5`, return *the sum of all paths from the root towards the leaves*.

It is **guaranteed** that the given array represents a valid connected binary tree.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/30/pathsum4-1-tree.jpg](https://assets.leetcode.com/uploads/2021/04/30/pathsum4-1-tree.jpg)

```
Input: nums = [113,215,221]
Output: 12
Explanation: The tree that the list represents is shown.
The path sum is (3 + 5) + (3 + 1) = 12.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/30/pathsum4-2-tree.jpg](https://assets.leetcode.com/uploads/2021/04/30/pathsum4-2-tree.jpg)

```
Input: nums = [113,221]
Output: 4
Explanation: The tree that the list represents is shown.
The path sum is (3 + 1) = 4.

```

**Constraints:**

- `1 <= nums.length <= 15`
- `110 <= nums[i] <= 489`
- `nums` represents a valid binary tree with depth less than `5`.

```cpp
class Solution {
public:
    int pathSum(vector<int>& nums) {
        unordered_map<int,int> kv;
        for(auto x: nums){
            int k=x/10, v=x%10;
            kv[k]=v;
        }
        //top down - with backtracking
        int res=0;
        int sum=0;
        function<void(int)> helper = [&](int node){
            sum+=kv[node];
            int d=node/10, p=node%10;
            int l=(d+1)*10 + p*2-1;
            int r=(d+1)*10 + p*2;
            if(!kv.count(l) && !kv.count(r)){
                res+=sum;
                sum-=kv[node];
                return;
            }
            if(kv.count(l)){ helper(l); }
            if(kv.count(r)){ helper(r); }
            sum-=kv[node];
        };
        helper(nums[0]/10);
        return res;
    }
};

/*
================================
dXX: depth: 1~4
XdX: position: 1~8
XXd: divalue: 0~9
depth <=5
return sum of all path from root to leaves
nums is ascending
================================
nums:
    len: 1~15
    val: 110~489
    valid
================================
d -> d+1
    l: 2*p-1
    r: 2*p
*/
```