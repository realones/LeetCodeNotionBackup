# [lint] Swap Two Nodes in Linked List

#: 511
Difficult: Medium
Tags: LinkedList
link: https://www.lintcode.com/problem/511/
Priority: Low
Created time: July 29, 2022 10:58 PM
Last edited time: October 16, 2023 5:02 PM
practicedTimes: 1
source: jiuzhang, 算初LinkedList&Array

**Description**

Given a linked list and two values `v1` and `v2`. Swap the two nodes in the linked list with values `v1` and `v2`. It's guaranteed there is no duplicate values in the linked list. If v1 or v2 does not exist in the given linked list, do nothing.

You should swap the two nodes with values `v1` and `v2`. Do not directly swap the values of the two nodes.

**Example**

**Example 1:**

```
Input: 1->2->3->4->null, v1 = 2, v2 = 4
Output: 1->4->3->2->null

```

**Example 2:**

```
Input: 1->null, v1 = 2, v2 = 1
Output: 1->null

```

difference senario:

1. node1->node2 or node2->node1
2. they are separate

```cpp
/**
 * Definition of singly-linked-list:
 * class ListNode {
 * public:
 *     int val;
 *     ListNode *next;
 *     ListNode(int val) {
 *        this->val = val;
 *        this->next = NULL;
 *     }
 * }
 */

class Solution {
public:
    /**
     * @param head: a ListNode
     * @param v1: An integer
     * @param v2: An integer
     * @return: a new head of singly-linked list
     */
    ListNode* swapNodes(ListNode *head, int v1, int v2) {
        // if same or !head or not found
        if(!head || v1==v2) return head;
        
        ListNode* dummy=new ListNode(0); dummy->next=head;
        ListNode* pre1=NULL, *pre2=NULL;
        for(ListNode* pre=dummy;pre->next;pre=pre->next){
            if(pre->next->val==v1 || pre->next->val==v2){
                pre1==NULL?pre1=pre:pre2=pre;
            }
        }
        if(!pre1 || !pre2) return head;
        ListNode *cur1=pre1->next, *nxt1=cur1->next;
        ListNode *cur2=pre2->next, *nxt2=cur2->next;
        //swap
        //if near by
        if(cur1->next==cur2){
            pre1->next=cur2;
            cur2->next=cur1;
            cur1->next=nxt2;
        }
        //if seperate
        else{
            pre1->next=cur2;cur2->next=nxt1;
            pre2->next=cur1;cur1->next=nxt2;
        }

        return dummy->next;
    }
};
```