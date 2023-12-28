# Odd Even Linked List

#: 328
Difficult: Medium
Tags: LinkedList
link: https://leetcode.com/problems/odd-even-linked-list/
Priority: Medium
Created time: July 19, 2023 7:06 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given the `head` of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return *the reordered list*.

The **first** node is considered **odd**, and the **second** node is **even**, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in `O(1)` extra space complexity and `O(n)` time complexity.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)

```
Input: head = [1,2,3,4,5]
Output: [1,3,5,2,4]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)

```
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]

```

**Constraints:**

- The number of nodes in the linked list is in the range `[0, 104]`.
- `106 <= Node.val <= 106`

solution new:

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
    ListNode* oddEvenList(ListNode* head) {
        if(!head || !head->next) return head;
        ListNode* dummyOdd=new ListNode(0), *preOdd=dummyOdd;
        ListNode* dummyEven=new ListNode(0), *preEven=dummyEven;
        bool odd=true;
        for(auto cur=head; cur; cur=cur->next){
            if(odd) {preOdd->next=cur; preOdd=preOdd->next;}
            else {preEven->next=cur; preEven=preEven->next;}
            odd=!odd;
        }
        preOdd->next=NULL;
        preEven->next=NULL;
        preOdd->next=dummyEven->next;
        return dummyOdd->next;
    }
}
```

solution old:

我开不太懂我之前咋做的了

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(!head || !head->next) return head;
        
        ListNode *odd=head, *evenhead=head->next, *even=evenhead;
        while(even && even->next){
            odd->next=odd->next->next; odd=odd->next;
            even->next=even->next->next; even=even->next;
        }
        odd->next=evenhead;
        return head;
    }
};
```