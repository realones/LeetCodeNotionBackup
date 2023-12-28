# Reverse Linked List

#: 206
Difficult: Easy
Tags: LinkedList, reverseAll
link: https://leetcode.com/problems/reverse-linked-list
Priority: Low
Created time: July 29, 2022 4:03 PM
Last edited time: October 13, 2023 1:04 PM
practicedTimes: 1
source: HuaHua, LC_75, jiuzhang, 算初LinkedList&Array

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]

```

**Example 3:**

```
Input: head = []
Output: []

```

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(!head) return NULL;
        
        ListNode* pre=NULL, *cur=head;
        while(cur){
            ListNode* nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        return pre;
    }
};
```