# Balance a Binary Search Tree

#: 1382
Difficult: Medium
Tags: DFS, Divide and Conquer, Greedy, Tree
link: https://leetcode.com/problems/balance-a-binary-search-tree/
Priority: Pass
Created time: June 21, 2023 8:20 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given the `root` of a binary search tree, return *a **balanced** binary search tree with the same node values*. If there is more than one answer, return **any of them**.

A binary search tree is **balanced** if the depth of the two subtrees of every node never differs by more than `1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg](https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg)

```
Input: root = [1,null,2,null,3,null,4,null,null]
Output: [2,1,3,null,null,null,4]
Explanation: This is not the only correct answer, [3,1,4,null,2] is also correct.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg](https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,1,3]

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `1 <= Node.val <= 105`

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* balanceBST(TreeNode* root) {
       //traverse to put into vector[idx]=nodeVal
        vector<int> arr;
        stack<TreeNode*> stk;
        pushLeft(stk,root);
        while(!stk.empty()){
            TreeNode* cur=stk.top(); stk.pop();
            arr.push_back(cur->val);
            if(cur->right){pushLeft(stk,cur->right);}
        }
       //build bst
       return buildBST(arr,0,arr.size()-1);
    }

    TreeNode* buildBST(vector<int>& arr, int start, int end){
        if(start>end) return NULL;
        int mid=start+(end-start)/2;

        TreeNode* node=new TreeNode(arr[mid]);
        node->left=buildBST(arr, start, mid-1);
        node->right=buildBST(arr, mid+1, end);
        return node;
    }

    void pushLeft(stack<TreeNode*>& stk, TreeNode* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```