# [lint] Convert Binary Tree to Linked Lists by Depth

#: 242
Difficult: Easy
Tags: BFS, Tree
link: https://www.lintcode.com/problem/242/
Priority: Pass
Created time: July 17, 2022 11:52 PM
Last edited time: October 13, 2023 1:03 PM
practicedTimes: 1
source: jiuzhang, 算初BFS

**Description**

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists).

*Contact me on wechat to get **Amazon、Google** requent Interview questions . (wechat id : **jiuzhang0607**)*

**Example**

**Example 1:**

```
Input: {1,2,3,4}
Output: [1->null,2->3->null,4->null]
Explanation:
        1
       / \
      2   3
     /
    4

```

**Example 2:**

```
Input: {1,#,2,3}
Output: [1->null,2->null,3->null]
Explanation:
    1
     \
      2
     /
    3

```

```cpp
public:
    /**
     * @param root the root of binary tree
     * @return a lists of linked list
     */
    vector<ListNode*> binaryTreeToLists(TreeNode* root) {
        vector<ListNode*> res;
        if(!root) return res;
        
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            ListNode* dummy=new ListNode(0);
            ListNode* pre=dummy;
            for(int i=0;i<sz;i++){
                TreeNode *cur=q.front();q.pop();
                pre->next=new ListNode(cur->val); pre=pre->next;
                if(cur->left){q.push(cur->left);}
                if(cur->right){q.push(cur->right);}
            }
            res.push_back(dummy->next);
            
        }
        
        return res;
    }
};
```