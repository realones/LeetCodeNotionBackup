# Insert into a Sorted Circular Linked List

#: 708
Difficult: Medium
Tags: LinkedList
link: https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list/description/
Priority: Medium
Status: HardToImplement
Created time: December 3, 2023 2:22 AM
Last edited time: December 3, 2023 2:23 AM
source: facebookFreq

Given a Circular Linked List node, which is sorted in non-descending order, write a function to insert a value `insertVal` into the list such that it remains a sorted circular list. The given node can be a reference to any single node in the list and may not necessarily be the smallest value in the circular list.

If there are multiple suitable places for insertion, you may choose any place to insert the new value. After the insertion, the circular list should remain sorted.

If the list is empty (i.e., the given node is `null`), you should create a new single circular list and return the reference to that single node. Otherwise, you should return the originally given node.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/01/19/example_1_before_65p.jpg](https://assets.leetcode.com/uploads/2019/01/19/example_1_before_65p.jpg)

```
Input: head = [3,4,1], insertVal = 2
Output: [3,4,1,2]
Explanation: In the figure above, there is a sorted circular list of three elements. You are given a reference to the node with value 3, and we need to insert 2 into the list. The new node should be inserted between node 1 and node 3. After the insertion, the list should look like this, and we should still return node 3.

```

![https://assets.leetcode.com/uploads/2019/01/19/example_1_after_65p.jpg](https://assets.leetcode.com/uploads/2019/01/19/example_1_after_65p.jpg)

**Example 2:**

```
Input: head = [], insertVal = 1
Output: [1]
Explanation: The list is empty (given head isnull). We create a new single circular list and return the reference to that single node.

```

**Example 3:**

```
Input: head = [1], insertVal = 0
Output: [1,0]

```

**Constraints:**

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `106 <= Node.val, insertVal <= 106`

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;

    Node() {}

    Node(int _val) {
        val = _val;
        next = NULL;
    }

    Node(int _val, Node* _next) {
        val = _val;
        next = _next;
    }
};
*/

class Solution {
public:
    Node* insert(Node* head, int insertVal) {
        Node *node= new Node(insertVal);
        //!head
        if(!head){
            node->next=node;
            return node;
        }
        bool canVisitHead=true;//to process head node only once
        Node* pre=NULL;//larget node <= insertVal
        Node* maxNode=NULL;//max val node
        int maxVal=INT_MIN;
        int preVal=INT_MIN;
        for(Node* cur=head; cur!=head || canVisitHead; cur=cur->next){
            //get max node
            if(maxVal<cur->val){
                maxVal=cur->val;
                maxNode=cur;
            }
            //get largest node <= insertVal
            if(cur->val <= insertVal && cur->val>=preVal){
                preVal=cur->val;
                pre=cur;
            }
            if(pre!=NULL && cur->val < preVal) break;
            canVisitHead=false;
        }
        if(pre==NULL){
            node->next=maxNode->next;
            maxNode->next=node;
        }
        else{
            node->next=pre->next;
            pre->next=node;
        }
        return head;
    }
};
```