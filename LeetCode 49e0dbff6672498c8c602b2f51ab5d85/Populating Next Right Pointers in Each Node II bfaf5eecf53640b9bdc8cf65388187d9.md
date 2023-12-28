# Populating Next Right Pointers in Each Node II

#: 117
Difficult: Medium
Tags: DFS, Tree
link: https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/
Priority: Medium
Created time: June 6, 2023 8:07 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given a binary tree

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}

```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/02/15/117_sample.png](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation:Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

```

**Example 2:**

```
Input: root = []
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 6000]`.
- `100 <= Node.val <= 100`

```cpp
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root) return;
        
        TreeLinkNode *parent1=root;
        TreeLinkNode *dummy=new TreeLinkNode(0);//dummy node used in each layer
        while(parent1){
            TreeLinkNode *pre=dummy;
            for(TreeLinkNode *p=parent1;p;p=p->next){
                if(p->left){pre->next=p->left; pre=pre->next;}
                if(p->right){pre->next=p->right; pre=pre->next;}
            }
            parent1=dummy->next;
            dummy->next=NULL;
        }
    }
};
```