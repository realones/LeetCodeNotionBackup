# Count Complete Tree Nodes

#: 222
Difficult: Medium
Tags: BS_todo, DFS, Tree
link: https://leetcode.com/problems/count-complete-tree-nodes/
Priority: Medium
Status: More Solution
Created time: June 5, 2023 4:39 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/14/complete.jpg](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

```
Input: root = [1,2,3,4,5,6]
Output: 6

```

**Example 2:**

```
Input: root = []
Output: 0

```

**Example 3:**

```
Input: root = [1]
Output: 1

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5 * 104]`.
- `0 <= Node.val <= 5 * 104`
- The tree is guaranteed to be **complete**.

Solution BS:

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
    int countNodes(TreeNode* root) {
        if(!root) return 0;
        //get 0 based height
        int h=0;
        TreeNode* node=root;
        while(node->left){
            h++;
            node=node->left;//validation
        }
        //check last row node cnt
        int l=1, r=pow(2,h);
        while(l<r){
            int m=l+((long long)r-l+1)/2;
            if(!valid(root,h,m)){r=m-1;}
            else{l=m;}
        }
        //up side + last row
        return pow(2,h)-1+r;
    }
    
    bool valid(TreeNode* root, int height, int cnt){
        //find seq from root to target leave
        vector<int> seq;
        int idx=cnt-1;
        for(int i=0;i<height;i++){
            seq.push_back(idx);
            idx/=2;
        }
        reverse(seq.begin(),seq.end());
        
        for(int i=0; i<height; i++){
            root = seq[i]%2==0 ? root->left : root->right;
        }
        return root != NULL;
    }
};
```

Solution DFS:

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(!root) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```