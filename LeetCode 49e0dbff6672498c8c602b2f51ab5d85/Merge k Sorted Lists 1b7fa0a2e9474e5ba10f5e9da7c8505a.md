# Merge k Sorted Lists

#: 23
Difficult: Hard
Tags: Divide and Conquer, LinkedList, MergeSort, PQ
link: https://leetcode.com/problems/merge-k-sorted-lists/
Priority: Medium
Created time: July 16, 2022 11:50 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, ticktokFreq, 算初Tree Divide&Conquer

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it.*

**Example 1:**

```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

```

**Example 2:**

```
Input: lists = []
Output: []

```

**Example 3:**

```
Input: lists = [[]]
Output: []

```

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

方法一:使用 PriorityQueue
方法二:类似归并排序的分治算法
方法三:自底向上的两两归并算法
时间复杂度均为 O(NlogK)

Solution:

1. PQ 

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

class MyComp{
public:
    bool operator()(ListNode* left, ListNode* right){
        return left->val > right->val;
    }
};

class Solution {
public:
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    ListNode *mergeKLists(vector<ListNode*>& lists) {
        if(lists.size()==NULL) return NULL;

        priority_queue<ListNode*,vector<ListNode *>,MyComp> q;
        for(auto n:lists){
            if(n!=NULL) q.push(n);
        }

        ListNode* root=new ListNode(0);
        ListNode* pre=root;

        while(!q.empty()){
            ListNode* cur=q.top();q.pop();
            if(cur->next!=NULL){q.push(cur->next);}

            pre->next=cur;
            pre=cur;
        }

        return root->next;
    }
};
```

  2. Sort

```cpp
class Solution {
public:
    ListNode *mergeKLists(vector<ListNode*>& lists) {
        if(lists.size()==0) return NULL;

        return mergeSort(lists,0,lists.size()-1);
    }

    ListNode* mergeSort(vector<ListNode*>& lists,int start,int end){
        if(start==end) return lists[start];

        int mid= start+(end-start)/2;
        ListNode* left=mergeSort(lists,start,mid);
        ListNode* right=mergeSort(lists,mid+1,end);
        return merge(left,right);
    }

    ListNode* merge(ListNode* left, ListNode* right){
        ListNode* root=new ListNode(0);
        ListNode* pre=root;
        while(left!=NULL && right!=NULL){
            if(left->val < right->val){
                pre->next=left;
                left=left->next;
                pre=pre->next;
            }
            else{
                pre->next=right;
                right=right->next;
                pre=pre->next;
            }
        }
        if(left!=NULL){pre->next=left;}
        if(right!=NULL){pre->next=right;}

        return root->next;
    }
};
```