# Double a Number Represented as a Linked List

#: 2816
Difficult: Medium
Tags: LinkedList, Math, stack
link: https://leetcode.com/problems/double-a-number-represented-as-a-linked-list/
Priority: Pass
Status: More Solution
Created time: August 12, 2023 11:03 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given the `head` of a **non-empty** linked list representing a non-negative integer without leading zeroes.

Return *the* `head` *of the linked list after **doubling** it*.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/05/28/example.png](https://assets.leetcode.com/uploads/2023/05/28/example.png)

```
Input: head = [1,8,9]
Output: [3,7,8]
Explanation: The figure above corresponds to the given linked list which represents the number 189. Hence, the returned linked list represents the number 189 * 2 = 378.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/05/28/example2.png](https://assets.leetcode.com/uploads/2023/05/28/example2.png)

```
Input: head = [9,9,9]
Output: [1,9,9,8]
Explanation: The figure above corresponds to the given linked list which represents the number 999. Hence, the returned linked list reprersents the number 999 * 2 = 1998.

```

**Constraints:**

- The number of nodes in the list is in the range `[1, 104]`
- `0 <= Node.val <= 9`
- The input is generated such that the list represents a number that does not have leading zeros, except the number `0` itself.

Solution lee215:

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
    ListNode* doubleIt(ListNode* head) {
        if (head->val > 4)
            head = new ListNode(0, head);
        for(ListNode* node = head; node; node = node->next) {
            node->val = (node->val * 2) % 10;
            if (node->next && node->next->val > 4)
                node->val++;
        }
        return head;
    }
};
/*
from lee215

Intuition
We double the value of every node,
If node.next.val > 4,
there will be carry from node.next.

Explanation
Iterate every node,
double the value node.val = (node.val * 2) % 10,
and check if there is a carry from node.next.

Complexity
Time O(n)
Space O(1)

*/
```

Solution stk:

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
    ListNode* doubleIt(ListNode* head) {
        stack<int> stk;
        for(ListNode* cur=head; cur; cur=cur->next){
            stk.push(cur->val);
        }
        ListNode* last = NULL;
        int inc=0;
        while(!stk.empty()){
            int cur=stk.top(); stk.pop();
            int val=(cur*2+inc)%10;
            inc=(cur*2+inc)/10;
            ListNode* node =new ListNode(val);
            node->next = last;
            last=node;
        }
        if(inc){
            ListNode* node =new ListNode(inc);
            node->next = last;
            last=node;
        }
        return last;
    }
}
```