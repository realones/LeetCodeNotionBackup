# Remove Nth Node From End of List

#: 19
Difficult: Medium
Tags: 2pt_SameDir_sameSpeed, LinkedList
link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/
Priority: Pass
Created time: August 20, 2022 1:35 PM
Last edited time: October 22, 2023 2:03 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算高TwoPointers&PQ

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

```

**Example 2:**

```
Input: head = [1], n = 1
Output: []

```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]

```

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int offset=n+1;
        ListNode* dummy=new ListNode(0); dummy->next=head;
        ListNode* pre1=dummy, *pre2=dummy;
        for(int i=0; i<offset; i++){
            pre2=pre2->next;
        }
        while(pre2){
            pre1=pre1->next;
            pre2=pre2->next;
        }
        pre1->next=pre1->next->next;
        return dummy->next;
    }
};
```