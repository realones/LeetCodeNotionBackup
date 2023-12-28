# Find Leaves of Binary Tree

#: 366
Difficult: Medium
Tags: DFS, Divide and Conquer, Tree
link: https://leetcode.com/problems/find-leaves-of-binary-tree/
Priority: Medium
Created time: October 3, 2022 10:48 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Given the `root` of a binary tree, collect a tree's nodes as if you were doing this:

- Collect all the leaf nodes.
- Remove all the leaf nodes.
- Repeat until the tree is empty.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/16/remleaves-tree.jpg](https://assets.leetcode.com/uploads/2021/03/16/remleaves-tree.jpg)

```
Input: root = [1,2,3,4,5]
Output: [[4,5,3],[2],[1]]
Explanation:
[[3,5,4],[2],[1]] and [[3,4,5],[2],[1]] are also considered correct answers since per each level it does not matter the order on which elements are returned.

```

**Example 2:**

```
Input: root = [1]
Output: [[1]]

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `100 <= Node.val <= 100`

DFS + Divide and Conquer

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
    vector<vector<int>> findLeaves(TreeNode* root) {
        //find height -> decide size of res vector
        //dfs -> put into res using reverse order in the res vector from the last one
        if(!root) return {};
        int height=getHeight(root);//1 based
        vector<vector<int>> res(height, vector<int>());
        
        dfs(root, res);
        reverse(res.begin(),res.end());
        return res;
    }
    
		//find idx: divide and conquer
		//push to res: dfs
    int dfs(TreeNode* root, vector<vector<int>>& res){//height: 0 based
        int n=res.size();
        if(!root) return 0;
        
        int lheight=dfs(root->left, res);
        int rheight=dfs(root->right, res);
        
        int idx;
        if(!lheight && !rheight){
            idx=n-1;
        }
        else if(lheight && rheight){//both
            idx=min(lheight,rheight)-1;
        }
        else if(lheight){//only lheight
            idx=lheight-1;
        }
        else{//only rheight
            idx=rheight-1;
        }
        res[idx].push_back(root->val);
        return idx;
    }
    
    int getHeight(TreeNode* root){
        if(!root) return 0;
        return max(getHeight(root->left), getHeight(root->right))+1;
    }
};
```