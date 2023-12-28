# Convert Sorted List to Binary Search Tree

#: 109
Difficult: Medium
Tags: Divide and Conquer, LinkedList, Tree
link: https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/
Priority: Medium
Created time: August 3, 2022 12:38 PM
Last edited time: October 19, 2023 3:51 PM
practicedTimes: 1
source: jiuzhang, 算初LinkedList&Array

Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/08/17/linked.jpg](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.

```

**Example 2:**

```
Input: head = []
Output: []

```

**Constraints:**

- The number of nodes in `head` is in the range `[0, 2 * 104]`.
- `105 <= Node.val <= 105`

找中点

```cpp
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(!head) return NULL;
        return helper(head,NULL);
    }
    
    TreeNode* helper(ListNode* begin, ListNode* end){//end is iterator end
        if(begin==end){return NULL;}
        ListNode* slow=begin, *fast=begin->next;
        while(fast!=end && fast->next!=end){
            slow=slow->next;
            fast=fast->next->next;
        }
        TreeNode* root=new TreeNode(slow->val);
        root->left=helper(begin,slow);
        root->right=helper(slow->next,end);
        return root;
    }
};
```