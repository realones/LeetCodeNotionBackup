# Vertical Order Traversal of a Binary Tree

#: 987
Difficult: Hard
Tags: BFS, DFS, Hash, Tree
link: https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/
Priority: High
Status: HardToImplement
Created time: October 22, 2022 11:41 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(row, col)`, its left and right children will be at positions `(row + 1, col - 1)` and `(row + 1, col + 1)` respectively. The root of the tree is at `(0, 0)`.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return *the **vertical order traversal** of the binary tree*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation:
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

```
Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

```
Input: root = [1,2,3,4,6,5,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation:
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

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
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        if(!root) return {};
        //use map to sort colomn, otherwise need to calculate left bound and right bound of colomn to define a vector to improve
        //use map for rows, can use vector to improve
        //for same coordinate, multi value even with same val can exist, -> multiset to sort
        map<int, map<int, multiset<int>>> mp;//colomn, row -> val
        //bfs
        queue<tuple<TreeNode*, int, int>> q;//node, colomn, row
        q.emplace(root,0,0);
        while(!q.empty()){
            auto [node, colomn, row]=q.front(); q.pop();
            mp[colomn][row].insert(node->val);
            if(node->left){q.emplace(node->left,colomn-1,row+1);}
            if(node->right){q.emplace(node->right,colomn+1,row+1);}
        }
        vector<vector<int>> res;
        for(auto& [_,row_valSet]: mp){
            vector<int> col;
            for(auto& [_, valSet]: row_valSet){
                col.insert(col.end(), valSet.begin(), valSet.end());
            }
            res.push_back(col);
        }
        return res;
    }
};
```