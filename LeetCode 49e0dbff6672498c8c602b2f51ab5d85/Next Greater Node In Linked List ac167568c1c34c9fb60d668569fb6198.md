# Next Greater Node In Linked List

#: 1019
Difficult: Medium
Tags: Array, LinkedList, monotonic, stack
link: https://leetcode.com/problems/next-greater-node-in-linked-list/description/
Priority: Low
Created time: September 11, 2023 5:45 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given the `head` of a linked list with `n` nodes.

For each node in the list, find the value of the **next greater node**. That is, for each node, find the value of the first node that is next to it and has a **strictly larger** value than it.

Return an integer array `answer` where `answer[i]` is the value of the next greater node of the `ith` node (**1-indexed**). If the `ith` node does not have a next greater node, set `answer[i] = 0`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext1.jpg](https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext1.jpg)

```
Input: head = [2,1,5]
Output: [5,5,0]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext2.jpg](https://assets.leetcode.com/uploads/2021/08/05/linkedlistnext2.jpg)

```
Input: head = [2,7,4,3,5]
Output: [7,0,5,5,0]

```

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 104`
- `1 <= Node.val <= 109`

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
using ii=pair<int,int>;

class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        if(!head) return {};

        int n=0;
        for(ListNode* cur=head; cur; cur=cur->next) n++;
        
        vector<int> res(n,0);
        stack<ii> stk;//<val,idx> dec
        int idx=0;
        for(ListNode* cur=head; cur; cur=cur->next, idx++){
            //trim
            while(!stk.empty() && stk.top().first < cur->val){
                res[stk.top().second]=cur->val;
                stk.pop();
            }
            //put
            stk.emplace(cur->val, idx);
        }
        return res;
    }
};

/*
next greator one
mono stk, dec

*/
```