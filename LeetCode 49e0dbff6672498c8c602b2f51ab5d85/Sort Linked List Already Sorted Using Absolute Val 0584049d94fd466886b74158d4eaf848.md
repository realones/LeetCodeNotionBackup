# Sort Linked List Already Sorted Using Absolute Values

#: 2046
Difficult: Medium
Tags: 2pt, LinkedList, Sort
link: https://leetcode.com/problems/sort-linked-list-already-sorted-using-absolute-values/
Priority: Low
Created time: November 17, 2023 1:26 AM
Last edited time: November 17, 2023 2:22 AM
practicedTimes: 1
source: ticktokFreq

Given the `head` of a singly linked list that is sorted in **non-decreasing** order using the **absolute values** of its nodes, return *the list sorted in **non-decreasing** order using the **actual values** of its nodes*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/10/17/image-20211017201240-3.png](https://assets.leetcode.com/uploads/2021/10/17/image-20211017201240-3.png)

```
Input: head = [0,2,-5,5,10,-10]
Output: [-10,-5,0,2,5,10]
Explanation:
The list sorted in non-descending order using the absolute values of the nodes is [0,2,-5,5,10,-10].
The list sorted in non-descending order using the actual values is [-10,-5,0,2,5,10].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/10/17/image-20211017201318-4.png](https://assets.leetcode.com/uploads/2021/10/17/image-20211017201318-4.png)

```
Input: head = [0,1,2]
Output: [0,1,2]
Explanation:
The linked list is already sorted in non-decreasing order.

```

**Example 3:**

```
Input: head = [1]
Output: [1]
Explanation:
The linked list is already sorted in non-decreasing order.

```

**Constraints:**

- The number of nodes in the list is the range `[1, 105]`.
- `5000 <= Node.val <= 5000`
- `head` is sorted in non-decreasing order using the absolute value of its nodes.

**Follow up:**

- Can you think of a solution with `O(n)` time complexity?

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
    ListNode* sortLinkedList(ListNode* head) {
        //split
        ListNode* posDummy=new ListNode(0), *posPre=posDummy;
        ListNode* negDummy=new ListNode(0), *negPre=negDummy;
        for(ListNode* cur=head; cur; cur=cur->next){
            if(cur->val<0){
                negPre->next = cur;
                negPre=negPre->next;
            }
            else{
                posPre->next = cur;
                posPre=posPre->next;
            }
        }
        negPre->next=NULL, posPre->next=NULL;
        if(!negDummy->next) return head;
        //reverse neg
        ListNode* negBack = negDummy->next;
        ListNode* pre=NULL, *cur=negDummy->next;
        while(cur){
            ListNode* nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        negDummy->next = pre;
        //link two list
        negBack->next = posDummy->next;
        return negDummy->next;
    }
};
```