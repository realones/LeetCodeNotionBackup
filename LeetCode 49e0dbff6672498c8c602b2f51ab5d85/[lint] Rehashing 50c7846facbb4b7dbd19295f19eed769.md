# [lint] Rehashing

#: 129
Difficult: Medium
Tags: Hash
link: https://www.lintcode.com/problem/rehashing
Priority: Pass
Created time: August 11, 2022 12:17 PM
Last edited time: October 21, 2023 9:50 PM
practicedTimes: 1
source: jiuzhang, 算初Hash&Heap

The size of the hash table is not determinate at the very beginning. If the total size of keys is too large (e.g. size >= capacity / 10), we should double the size of the hash table and rehash every keys. Say you have a hash table looks like below:

`size=3`, `capacity=4`

```
[null, 21, 14, null]
       ↓    ↓
       9   null
       ↓
      null

```

The hash function is:

```
int hashcode(int key, int capacity) {
    return key % capacity;
}

```

here we have three numbers, 9, 14 and 21, where 21 and 9 share the same position as they all have the same hashcode 1 (21 % 4 = 9 % 4 = 1). We store them in the hash table by linked list.

rehashing this hash table, double the capacity, you will get:

`size=3`, `capacity=8`

```
index:   0    1    2    3     4    5    6   7
hash : [null, 9, null, null, null, 21, 14, null]

```

Given the original hash table, return the new hash table after rehashing .

Example:
***Example 1***

```
Input : [null, 21->9->null, 14->null, null]
Output : [null, 9->null, null, null, null, 21->null, 14->null, null]

```

```cpp
class Solution {
public:
    /**
     * @param hashTable: A list of The first node of linked list
     * @return: A list of The first node of linked list which have twice size
     */
    vector<ListNode*> rehashing(vector<ListNode*> hashTable) {
        // write your code here
        if(hashTable.empty()) return vector<ListNode*>();

        int sz=hashTable.size()*2;
        vector<ListNode*> res(sz,NULL);
        for(auto cur:hashTable){
            while(cur!=NULL){
                int index=(cur->val % sz + sz) %sz;
                if(res[index]==NULL) res[index]=new ListNode(cur->val);
                else{
                    ListNode* tmp=res[index];
                    while(tmp->next!=NULL) tmp=tmp->next;
                    tmp->next=new ListNode(cur->val);
                }
                cur=cur->next;
            }
        }
        return res;

    }
};
```