# Linked List Components

#: 817
Difficult: Medium
Tags: Graph, Hash, LinkedList
link: https://leetcode.com/problems/linked-list-components/description/
Priority: Pass
Created time: June 27, 2023 11:03 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given the `head` of a linked list containing unique integer values and an integer array `nums` that is a subset of the linked list values.

Return *the number of connected components in* `nums` *where two values are connected if they appear **consecutively** in the linked list*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg)

```
Input: head = [0,1,2,3], nums = [0,1,3]
Output: 2
Explanation: 0 and 1 are connected, so [0, 1] and [3] are the two connected components.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg)

```
Input: head = [0,1,2,3,4], nums = [0,3,1,4]
Output: 2
Explanation: 0 and 1 are connected, 3 and 4 are connected, so [0, 1] and [3, 4] are the two connected components.

```

**Constraints:**

- The number of nodes in the linked list is `n`.
- `1 <= n <= 104`
- `0 <= Node.val < n`
- All the values `Node.val` are **unique**.
- `1 <= nums.length <= n`
- `0 <= nums[i] < n`
- All the values of `nums` are **unique**.

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
    int numComponents(ListNode* head, vector<int>& nums) {
        unordered_set<int> st(nums.begin(), nums.end());
        int res=0;
        int preVal=-1;
        for(ListNode* cur=head; cur; cur=cur->next){
            if(st.count(cur->val) && !st.count(preVal)) res++;
            preVal=cur->val;
        }
        return res;
    }
};
```