# Delete Node in a BST

#: 450
Difficult: Medium
Tags: 1Path, BST, Tree
link: https://leetcode.com/problems/delete-node-in-a-bst/
Priority: High
Status: HardToImplement, More Solution
Created time: July 15, 2022 12:17 AM
Last edited time: October 14, 2023 10:40 PM
practicedTimes: 1
source: HuaHua, LC_75, jiuzhang, 算初Tree Divide&Conquer

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return *the **root node reference** (possibly updated) of the BST*.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

```
Input: root = [5,3,6,2,4,null,7], key = 3
Output: [5,4,6,2,null,null,7]
Explanation: Given key to delete is 3. So we find the node with value 3 and delete it.
One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.

```

![https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

**Example 2:**

```
Input: root = [5,3,6,2,4,null,7], key = 0
Output: [5,3,6,2,4,null,7]
Explanation: The tree does not contain a node with value = 0.

```

**Example 3:**

```
Input: root = [], key = 0
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `105 <= Node.val <= 105`
- Each node has a **unique** value.
- `root` is a valid binary search tree.
- `105 <= key <= 105`

**Follow up:** Could you solve it with time complexity `O(height of tree)`?

Solution 2:

there are recursive easy solution, but below is better:

find parent, if node->right exist, find min of the node->right subtree and put it in the node position

[http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete.html](http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete.html)

```jsx
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(!root) return NULL;
        
        //find target and parent
        TreeNode* target=root;
        TreeNode *dummy=new TreeNode(INT_MAX); dummy->left=root;
        TreeNode* parent=dummy;
        while(target && (target->val!=key)){
            parent = target;
            if(target->val>key){target=target->left;}
            else{target=target->right;}
        }
        if(!target) return root; //not found target
        
        //if no child, delete
        if(!target->left && !target->right) {getChildRef(parent,target)=NULL; return dummy->left;}
        //if one child, move up
        if(!target->left) {getChildRef(parent,target)=target->right;return dummy->left;}
        if(!target->right) {getChildRef(parent,target)=target->left;return dummy->left;}
        //if two child, go right, then get the left_most
        //cut the left_most out by moving left_most->right up, put left_most to replace the target
        TreeNode* leftMost=target->right, *leftMostParent=target;
        while(leftMost->left){
            leftMostParent=leftMost;
            leftMost=leftMost->left;
        }
        getChildRef(leftMostParent,leftMost)=leftMost->right;
        //link left most to target
        getChildRef(parent,target)=leftMost;
        leftMost->left=target->left;
        leftMost->right=target->right;
        
        return dummy->left;
    }
    
    TreeNode*& getChildRef(TreeNode* parent, TreeNode* child){
        return (parent->val > child->val) ? parent->left : parent->right;
    }
    
};
```

```cpp
class Solution {
public:
    typedef pair<TreeNode*, TreeNode*> ParentChild;
    
    TreeNode* deleteNode(TreeNode* root, int value) {
        if(!root) return NULL;
        TreeNode* dummy = new TreeNode(INT_MAX);
        dummy->left = root;
        //find parent
        ParentChild pc = findToRemove(dummy, value);
        if(!pc.first) return root;
        //remove node
        removeNode2(pc.first, pc.second);
        
        return dummy->left;
    }
    
    void removeNode2(TreeNode* toRemoveParent, TreeNode* toRemove){
        if(!toRemove->right){//just move up the left
            getChildRef(toRemoveParent, toRemove) = toRemove->left;
        }
        //if there is a right 
        else{
            //find smallest of toRemove->right, cut it out
            ParentChild pc = findLeftMost(toRemove);
            TreeNode* leftMostParent = pc.first;
            TreeNode* leftMost = pc.second;
            
            getChildRef(leftMostParent, leftMost) = leftMost->right;
            
            //put leftMost in place of toRemove
            //link top
            getChildRef(toRemoveParent, toRemove) = leftMost;
            //link down
            leftMost->left = toRemove->left;
            leftMost->right = toRemove->right;
        }
    }
    
    ParentChild findToRemove(TreeNode* dummy, int value) {
        TreeNode* parent = dummy;
        TreeNode* cur = parent->left;
        while(cur){
            if(cur->val == value){return ParentChild(parent,cur);}
            parent = cur;
            cur = (cur->val >value) ? cur->left : cur->right;
        }
        return ParentChild(NULL, NULL);
    }
    
    TreeNode*& getChildRef(TreeNode* parent, TreeNode* child){
        return (parent->val > child->val) ? parent->left : parent->right;
    }
    
    ParentChild findLeftMost(TreeNode* toRemove) {
        TreeNode* parent = toRemove;
        TreeNode* cur = toRemove->right;
        while(cur->left){
            parent = cur;
            cur=cur->left;
        }
        return ParentChild(parent, cur);
    }
};
```