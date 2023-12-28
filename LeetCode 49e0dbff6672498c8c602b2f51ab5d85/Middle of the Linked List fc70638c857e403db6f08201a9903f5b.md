# Middle of the Linked List

#: 876
Difficult: Easy
Tags: 2pt_SameDir_FastDoubleSpeed
link: https://leetcode.com/problems/middle-of-the-linked-list/
Priority: Medium
Created time: August 22, 2022 12:54 AM
Last edited time: December 24, 2023 11:41 PM
practicedTimes: 1
source: googleFreq, jiuzhang, 算高TwoPointers&PQ

Given the `head` of a singly linked list, return *the middle node of the linked list*.

If there are two middle nodes, return **the second middle** node.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.

```

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow=head, *fast=head;
        while(fast && fast->next){
            slow=slow->next;
            fast=fast->next->next;
        }
        return slow;
    }
};
```