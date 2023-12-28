# Rank Transform of an Array

#: 1331
Difficult: Easy
Tags: Array, Hash, Sort
link: https://leetcode.com/problems/rank-transform-of-an-array/description/
Priority: Low
Status: toRevisit
Created time: December 3, 2023 4:35 PM
Last edited time: December 3, 2023 4:36 PM
source: facebookFreq

Given an array of integers `arr`, replace each element with its rank.

The rank represents how large the element is. The rank has the following rules:

- Rank is an integer starting from 1.
- The larger the element, the larger the rank. If two elements are equal, their rank must be the same.
- Rank should be as small as possible.

**Example 1:**

```
Input: arr = [40,10,20,30]
Output: [4,1,2,3]
Explanation: 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.
```

**Example 2:**

```
Input: arr = [100,100,100]
Output: [1,1,1]
Explanation: Same elements share the same rank.

```

**Example 3:**

```
Input: arr = [37,12,28,9,100,56,80,5,12]
Output: [5,3,4,2,8,6,7,1,3]

```

**Constraints:**

- `0 <= arr.length <= 105`
- `109 <= arr[i] <= 109`

comment: to implement w/o hash

```cpp
using ii=pair<int,int>;

class MyHashMap {
public:
    int M=1e4;
    vector<list<ii>> v;

    MyHashMap() {
        //in real situation we need to dynamiclly increase the size of v
        //e.g. sz*=2
        v.resize(M);
    }
    
    void put(int key, int value) {
        int idx=key%M;
        for(auto& [k,v]: v[idx]){
            if(k==key) {
                v=value; 
                return;
            }
        }
        v[idx].emplace_back(key,value);
    }
    
    int get(int key) {
        int idx=key%M;
        for(auto [k,v]: v[idx]){
            if(k==key) return v;
        }
        return -1;//not found
    }
    
    void remove(int key) {
        int idx=key%M;
        for(auto iter=v[idx].begin(); iter!=v[idx].end(); iter++){
            int k=iter->first;
            if(k==key) {
                v[idx].erase(iter);
                return;
            }
        }
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```