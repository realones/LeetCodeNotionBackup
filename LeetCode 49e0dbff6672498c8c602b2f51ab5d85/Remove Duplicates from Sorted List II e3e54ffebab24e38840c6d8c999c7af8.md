# Remove Duplicates from Sorted List II

#: 82
Difficult: Medium
Tags: 2pt, Hash, LinkedList
link: https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/
Priority: Medium
Status: More Solution
Created time: June 4, 2023 11:51 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list **sorted** as well*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

```
Input: head = [1,1,1,2,3]
Output: [2,3]

```

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.

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
    ListNode* deleteDuplicates(ListNode* head) {
        unordered_set<int> st;
        unordered_set<int> dupSt;
        for(ListNode* cur=head;cur;cur=cur->next){
            if(!st.count(cur->val)) st.insert(cur->val);
            else{
                dupSt.insert(cur->val);
            }
        }
        
        ListNode* dummy=new ListNode(0), *pre=dummy;
        for(ListNode* cur=head;cur;cur=cur->next){
            if(dupSt.count(cur->val)) continue;
            pre->next=cur;
            pre=pre->next;
        }
        pre->next=NULL;
        return dummy->next;
    }
};
```