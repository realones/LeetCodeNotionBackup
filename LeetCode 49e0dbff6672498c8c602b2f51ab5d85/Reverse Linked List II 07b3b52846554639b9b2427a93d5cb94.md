# Reverse Linked List II

#: 92
Difficult: Medium
Tags: LinkedList, reversePartial
link: https://leetcode.com/problems/reverse-linked-list-ii/
Priority: Low
Created time: July 29, 2022 4:03 PM
Last edited time: October 17, 2023 6:36 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初LinkedList&Array

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]

```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]

```

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**Follow up:**

Could you do it in one pass?

```cpp
revert part inside a list:
         ------------------------------
         -                           ->
...-->[left]   [lastRev]<--[   ]<-- [   ].     [nxt]-->...
                  -                           ->
                  ------------------------------
```

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* dummy=new ListNode(0); dummy->next=head;
        ListNode* leftNode = dummy;
        for(int i=0; i<left-1; i++){
            leftNode=leftNode->next;
        }
        int times=right-left;
        reverse(leftNode, times);
        return dummy->next;
    }
    
    //left: input - left side node of the head of list to be reverted
    //times: input - how many steps to revert, e.g. times=1, 2 nodes reverted
    void reverse(ListNode*& left, int times){
        //left and lastRev fixed node, nxt moving forward
        ListNode* lastRev = left->next;
        for(int i=0; i<times; i++){
            ListNode* nxt=lastRev->next;
            lastRev->next=nxt->next;
            nxt->next=left->next;
            left->next=nxt;
        }
        left=lastRev;
    }
};

/*
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if(head==NULL || m>=n) return head;
        ListNode* dummy=new ListNode(0); dummy->next=head;
        ListNode* pre=dummy, *start=head;
        for(int i=0;i<m-1;i++){
            pre=pre->next;
        }
        start=pre->next;
        for(int i=0;i<n-m;i++){
            ListNode* tmp=start->next;
            start->next=tmp->next;
            tmp->next=pre->next;
            pre->next=tmp;
        }
        
        return dummy->next;
    }
};
 * */
```