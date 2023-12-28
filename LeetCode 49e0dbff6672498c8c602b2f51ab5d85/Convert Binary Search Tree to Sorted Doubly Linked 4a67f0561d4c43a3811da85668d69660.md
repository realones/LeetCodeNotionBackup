# Convert Binary Search Tree to Sorted Doubly Linked List

#: 426
Difficult: Medium
Tags: Iterate, Tree, changeNotAffectIteration
link: https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list
Priority: Low
Created time: July 12, 2022 10:02 PM
Last edited time: October 13, 2023 12:48 PM
practicedTimes: 1
source: jiuzhang, 算初Tree Divide&Conquer

Convert a **Binary Search Tree** to a sorted **Circular Doubly-Linked List** in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)

```
Input: root = [4,2,5,1,3]

Output: [1,2,3,4,5]

Explanation: The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.

```

![https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

![https://assets.leetcode.com/uploads/2018/10/12/bstdllreturnbst.png](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturnbst.png)

**Example 2:**

```
Input: root = [2,1,3]
Output: [1,2,3]

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `1000 <= Node.val <= 1000`
- All the values of the tree are **unique**.

可以看看recursive的解法

这里inorder iterate一遍直接解决

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/

class Solution {
public:
    Node* last;
        
    Node* treeToDoublyList(Node* root) {
        if(!root) return NULL;
        Node* dummy = new Node(0);
        last=dummy;
        treeToDoublyList2(root);
        last->right=dummy->right;
        dummy->right->left=last;
        return dummy->right;
    }
        
    void treeToDoublyList2(Node* root) {
        if(!root) return;
        stack<Node*> stk;
        pushLeft(stk, root);
        while(!stk.empty()){
            Node* cur = stk.top(); stk.pop();
            
            cur->left=last;
            last->right=cur;
            last=cur;
            
            pushLeft(stk, cur->right);
        }
        
    }
    
    void pushLeft(stack<Node*> &stk, Node* node){
        while(node){
            stk.push(node);
            node=node->left;
        }
    }
};
```