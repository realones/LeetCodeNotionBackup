# Add Two Numbers II

#: 445
Difficult: Medium
Tags: LinkedList, Math, stack
link: https://leetcode.com/problems/add-two-numbers-ii/description/
Priority: Medium
Status: HardToImplement
Created time: June 29, 2023 3:29 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg](https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg)

```
Input: l1 = [7,2,4,3], l2 = [5,6,4]
Output: [7,8,0,7]

```

**Example 2:**

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [8,0,7]

```

**Example 3:**

```
Input: l1 = [0], l2 = [0]
Output: [0]

```

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

**Follow up:** Could you solve it without reversing the input lists?

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int sz1=0,sz2=0; 
        for(ListNode* l=l1;l;l=l->next) sz1++;
        for(ListNode* l=l2;l;l=l->next) sz2++;
        int sz=max(sz1,sz2);

        //stk: from left to right
        stack<int> stk;
        for(int i=0; i<sz; i++){
            int v1=0;
            if(i>=sz-sz1){ v1=l1->val; l1=l1->next; }
            int v2=0;
            if(i>=sz-sz2){ v2=l2->val; l2=l2->next; }
            stk.push(v1+v2);
        }

        //calcu: from top to bot, add to res list
        ListNode* head=NULL;
        while(!stk.empty()){
            int val=stk.top(); stk.pop();
            int push=val/10; val=val%10;
            if(push==1) {
                if(stk.empty()) stk.push(1);
                else stk.top()+=1;
            }
            ListNode* node = new ListNode(val);
            node->next=head;
            head=node;
        }
        return head;
    }
}
```