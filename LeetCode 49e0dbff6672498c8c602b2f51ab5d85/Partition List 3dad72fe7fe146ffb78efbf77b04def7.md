# Partition List

#: 86
Difficult: Medium
Tags: LinkedList
link: https://leetcode.com/problems/partition-list/
Priority: Low
Created time: July 29, 2022 4:03 PM
Last edited time: October 17, 2023 3:02 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初LinkedList&Array

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/04/partition.jpg](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]

```

**Example 2:**

```
Input: head = [2,1], x = 2
Output: [1,2]

```

**Constraints:**

- The number of nodes in the list is in the range `[0, 200]`.
- `100 <= Node.val <= 100`
- `200 <= x <= 200`

reverse merge list

```cpp
class Solution {
public:
    /*
     * @param head: The first node of linked list
     * @param x: An integer
     * @return: A ListNode
     */
    ListNode * partition(ListNode * head, int x) {
        ListNode * ldummy=new ListNode(0), *lpre=ldummy;
        ListNode * rdummy=new ListNode(0), *rpre=rdummy;

        for(ListNode* cur=head;cur;cur=cur->next){
            if(cur->val<x){lpre->next=cur; lpre=lpre->next;}
            else{rpre->next=cur; rpre=rpre->next;}
        }
        rpre->next=NULL;
        lpre->next=rdummy->next;
        return ldummy->next;
    }
};
```