# Add Two Numbers

#: 2
Difficult: Medium
Tags: LinkedList, Math, simulation
link: https://leetcode.com/problems/add-two-numbers/
Priority: Low
Created time: June 4, 2023 11:44 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

ou are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.

```

**Example 2:**

```
Input: l1 = [0], l2 = [0]
Output: [0]

```

**Example 3:**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]

```

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

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
	ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
		ListNode *ans = new ListNode(0), *p = ans;
		int cur = 0;
		while (l1 || l2 || cur)
		{
			p->val = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + cur;
			cur = p->val / 10;
			p->val %= 10;
			if (l1) l1 = l1->next;
			if (l2) l2 = l2->next;
			if (l1 || l2 || cur)
				p->next = new ListNode(0);
			p = p->next;
		}
		return ans;
	}
};
```