# Copy List with Random Pointer

#: 138
Difficult: Medium
Tags: Graph, Hash, LinkedList
link: https://leetcode.com/problems/copy-list-with-random-pointer/
Priority: Low
Created time: July 29, 2022 10:58 PM
Last edited time: October 16, 2023 5:02 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初Hash&Heap, 算初LinkedList&Array

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a **[deep copy](https://en.wikipedia.org/wiki/Object_copying#Deep_copy)** of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/12/18/e1.png](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/12/18/e2.png](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2019/12/18/e3.png](https://assets.leetcode.com/uploads/2019/12/18/e3.png)

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]

```

**Constraints:**

- `0 <= n <= 1000`
- `104 <= Node.val <= 104`
- `Node.random` is `null` or is pointing to some node in the linked list.

记录randomPoint对应关系
solution1 map
solution2 原链表ABC… ->double->AABBCC...，然后分离

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return NULL;
        //dup
        for(Node* cur=head; cur; cur=cur->next->next){
            Node* dup=new Node(cur->val);
            Node* nxt=cur->next;
            cur->next=dup;
            dup->next=nxt;
        }
        for(Node* cur=head; cur; cur=cur->next->next){
            Node* dup=cur->next;
            if(!cur->random) continue;
            dup->random=cur->random->next;
        }
        //cut
        Node* dummy = new Node(0), *pre=dummy;
        for(Node* cur=head; cur; cur=cur->next){
            Node* dup=cur->next;
            cur->next=cur->next->next;
            pre->next=dup;
            pre=pre->next;
        }
        return dummy->next;
    }
};
```

```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return NULL;
        copyNext(head);
        copyRandom(head);
        return splitList(head);
    }
    
private:
    void copyNext(Node* node){
        while(node){
            Node *cp = new Node(node->val);
            cp->next = node->next;
            node->next=cp;
            node=node->next->next;
        }
    }
    
    void copyRandom(Node* node){
        while(node){
            Node* cp=node->next;
            if(node->random){
                cp->random = node->random->next;
            }
            node=node->next->next;
        }
    }
    
    Node* splitList(Node* node){
        Node *cpHead = node->next;
        while(node){
            Node* cp = node->next;
            node->next=node->next->next;
            if(cp->next){
                cp->next=cp->next->next;
            }
            node = node->next;
        }
        return cpHead;
    }
};

/*
    RandomListNode *copyRandomList(RandomListNode *head) {
        // write your code here
        unordered_map<RandomListNode*, RandomListNode*> old2new;
        RandomListNode* dummy = new RandomListNode(-1);
        RandomListNode* tmp = head;
        RandomListNode* curr = dummy;
        while (tmp) {
            RandomListNode* newNode = new RandomListNode(tmp->label);
            old2new[tmp] = newNode;
            curr->next = newNode;
            curr = curr -> next;
            tmp = tmp->next;
        }
        tmp = head;
        while (tmp) {
            if (tmp->random) {
                old2new[tmp]->random = old2new[tmp->random];
            }
            tmp = tmp-> next;
        }
        return dummy->next;
    }
*/
```