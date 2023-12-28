# Reverse Nodes in k-Group

#: 25
Difficult: Hard
Tags: LinkedList, reversePartial
link: https://leetcode.com/problems/reverse-nodes-in-k-group/
Priority: Low
Created time: July 29, 2022 4:03 PM
Last edited time: October 17, 2023 6:36 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初LinkedList&Array

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return *the modified list*.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

```

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?

```cpp
 class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy=new ListNode(0); dummy->next=head;
        int sz=0;
        for(ListNode* cur=head;cur;cur=cur->next){sz++;}
        int groupCnt=sz/k;
        ListNode* left=dummy;
        for(int i=0; i<groupCnt; i++){
            reverse(left,k-1);
        }
        return dummy->next;
    }
    
    void reverse(ListNode*& left, int times){
        ListNode* lastRev=left->next;
        for(int i=0; i<times; i++){
            ListNode* nxt=lastRev->next;
            lastRev->next=nxt->next;
            nxt->next=left->next;
            left->next=nxt;
        }
        left=lastRev;
    }
};
```