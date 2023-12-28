# Split Linked List in Parts

#: 725
Difficult: Medium
Tags: LinkedList, simulation
link: https://leetcode.com/problems/split-linked-list-in-parts/
Priority: Low
Created time: June 27, 2023 10:59 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given the `head` of a singly linked list and an integer `k`, split the linked list into `k` consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return *an array of the* `k` *parts*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

```
Input: head = [1,2,3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but its string representation as a ListNode is [].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

```
Input: head = [1,2,3,4,5,6,7,8,9,10], k = 3
Output: [[1,2,3,4],[5,6,7],[8,9,10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.

```

**Constraints:**

- The number of nodes in the list is in the range `[0, 1000]`.
- `0 <= Node.val <= 1000`
- `1 <= k <= 50`

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
    vector<ListNode*> splitListToParts(ListNode* head, int k) {
        vector<ListNode*> res;
        int sz=0;
        for(ListNode* cur=head; cur; cur=cur->next) sz++;
        int baseLen=sz/k;
        int firstKth=sz%k;

        ListNode* dummy=new ListNode(0); 
        for(int i=0; i<k; i++){
            int len=baseLen;
            if(i+1<=firstKth) len++;

            dummy->next=head;
            ListNode* pre=dummy;
            for(int j=0; j<len; j++){
                pre=pre->next;
            }
            head=pre->next;
            pre->next=NULL;
            res.push_back(dummy->next);
        }
        return res;
    }
};
```