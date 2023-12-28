# Maximum Twin Sum of a Linked List

#: 2130
Difficult: Medium
Tags: 2pt_SameDir_diffSpeed, LinkedList, listMid, stack
link: https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list
Priority: Medium
Status: More Solution
Created time: July 8, 2023 3:17 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

In a linked list of size `n`, where `n` is **even**, the `ith` node (**0-indexed**) of the linked list is known as the **twin** of the `(n-1-i)th` node, if `0 <= i <= (n / 2) - 1`.

- For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.

The **twin sum** is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return *the **maximum twin sum** of the linked list*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)

```
Input: head = [5,4,2,1]
Output: 6
Explanation:
Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
There are no other nodes with twins in the linked list.
Thus, the maximum twin sum of the linked list is 6.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)

```
Input: head = [4,2,2,3]
Output: 7
Explanation:
The nodes with twins present in this linked list are:
- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.
Thus, the maximum twin sum of the linked list is max(7, 4) = 7.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)

```
Input: head = [1,100000]
Output: 100001
Explanation:
There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.

```

**Constraints:**

- The number of nodes in the list is an **even** integer in the range `[2, 105]`.
- `1 <= Node.val <= 105`

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
    int pairSum(ListNode* head) {//len: 2~1e5, val: 1~1e5
        //cut into two
        ListNode* slow=head, *fast=head->next;
        while(fast && fast->next){
            slow=slow->next;
            fast=fast->next->next;
        }
        ListNode* head2=slow->next;
        slow->next=NULL;
        //reverse head2
        ListNode* pre=NULL, *cur=head2;
        while(cur){
            ListNode* nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        head2=pre;
        //check res
        int res=0;
        while(head && head2){
            res=max(res, head->val + head2->val);
            head=head->next;
            head2=head2->next;
        }
        return res;
    }
};

/*
len is even
find mid, cut and reverse 2nd, add two list
*
```