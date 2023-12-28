# Convert Sorted Array to Binary Search Tree

#: 108
Difficult: Easy
Tags: Divide and Conquer, Tree
link: https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/
Priority: Pass
Created time: June 6, 2023 10:58 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given an integer array `nums` where the elements are sorted in **ascending order**, convert *it to a **height-balanced*** *binary search tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:

```

![https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg](https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg)

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/18/btree.jpg](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)

```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.

```

**Constraints:**

- `1 <= nums.length <= 104`
- `104 <= nums[i] <= 104`
- `nums` is sorted in a **strictly increasing** order.

```cpp
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int start=0,end=nums.size()-1;
        return makeTree(nums,start,end);
    }
    TreeNode* makeTree(vector<int>& nums,int start,int end){
        if(start>end) return NULL;
        int mid=start+(end-start)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=makeTree(nums,start,mid-1);
        root->right=makeTree(nums,mid+1,end);
        return root;
    }
};
```