# Kth Smallest Element in a BST

#: 230
Difficult: Medium
Tags: Iterate, Tree
link: https://leetcode.com/problems/kth-smallest-element-in-a-bst/
Priority: Pass
Created time: June 6, 2023 8:30 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3

```

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

```cpp
class Solution {
public:
    int cnt=0;
    int kthSmallest(TreeNode* root, int k) {
        int res=-1;
        int cnt=0;
        helper(root,k,cnt,res);//inorder
        return res;
    }
    
    void helper(TreeNode* root, int k, int& cnt, int &res){
        if(root==NULL) return;
        if(cnt>=k) return;
        helper(root->left,k,cnt,res);
        cnt++;
        if(cnt==k){res=root->val;}
        helper(root->right,k,cnt,res);
    }
};
```