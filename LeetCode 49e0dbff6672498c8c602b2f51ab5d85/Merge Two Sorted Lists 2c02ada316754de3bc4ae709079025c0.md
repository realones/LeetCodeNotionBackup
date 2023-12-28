# Merge Two Sorted Lists

#: 21
Difficult: Easy
Tags: LinkedList, mergeList
link: https://leetcode.com/problems/merge-two-sorted-lists
Priority: Pass
Created time: July 29, 2022 10:57 PM
Last edited time: October 16, 2023 4:58 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初LinkedList&Array

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]

```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []

```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]

```

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy=new ListNode(0), *pre=dummy;
        ListNode* cur1=list1, *cur2=list2;
        while(cur1&&cur2){
            if(cur1->val < cur2->val){
                pre->next=cur1;
                cur1=cur1->next;
                pre=pre->next;
            }
            else{
                pre->next=cur2;
                cur2=cur2->next;
                pre=pre->next;
            }
        }
        if(cur1){pre->next=cur1;}
        if(cur2){pre->next=cur2;}
        return dummy->next;
    }
};
```