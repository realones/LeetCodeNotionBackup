# LFU Cache

#: 460
Difficult: Hard
Tags: Design, Hash, LinkedList, Tree
link: https://leetcode.com/problems/lfu-cache/
Priority: Medium
Status: Dig More
Created time: August 11, 2022 6:06 PM
Last edited time: October 21, 2023 1:15 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Hash&Heap
related: All O`one Data Structure (All%20O%60one%20Data%20Structure%205ddb07ce09004136b2b3b972e0469fb8.md)

Design and implement a data structure for a [Least Frequently Used (LFU)](https://en.wikipedia.org/wiki/Least_frequently_used) cache.

Implement the `LFUCache` class:

- `LFUCache(int capacity)` Initializes the object with the `capacity` of the data structure.
- `int get(int key)` Gets the value of the `key` if the `key` exists in the cache. Otherwise, returns `1`.
- `void put(int key, int value)` Update the value of the `key` if present, or inserts the `key` if not already present. When the cache reaches its `capacity`, it should invalidate and remove the **least frequently used** key before inserting a new item. For this problem, when there is a **tie** (i.e., two or more keys with the same frequency), the **least recently used** `key` would be invalidated.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to `1` (due to the `put` operation). The **use counter** for a key in the cache is incremented either a `get` or `put` operation is called on it.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

```
Input
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

Explanation
// cnt(x) = the use counter for key x
// cache=[] will show the last used order for tiebreakers (leftmost element is  most recent)
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // return 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 2 is the LFU key because cnt(2)=1 is the smallest, invalidate 2.
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // Both 1 and 3 have the same cnt, but 1 is LRU, invalidate 1.
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // return -1 (not found)
lfu.get(3);      // return 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // return 4
                 // cache=[4,3], cnt(4)=2, cnt(3)=3

```

**Constraints:**

- `0 <= capacity <= 104`
- `0 <= key <= 105`
- `0 <= value <= 109`
- At most `2 * 105` calls will be made to `get` and `put`.

“for LFU, we can use a map of LRU, because we only remove key with frequency==1, dont need to keep sequence?” 
Wrong, e.g. when all element has freq = 2, then insert new element. Instead, we use min_freq to track the smallest freq. 

As a result, use map+min_freq similar to the List in LRU leetcode question. To keep some kind of sequence for remove last purpose.

```cpp
class LFUCache {
private:
	int cap;
	int min_freq;
	unordered_map<int, list<int>> freq_keyList_map; //front is most freq/recent one
	unordered_map<int, int> key_val_map;
	unordered_map<int, int> key_freq_map;
	unordered_map<int, list<int>::iterator> key_iter_map;

public:
	LFUCache(int capacity) {
		cap = capacity;
		min_freq = 0;
	}

	int get(int key) {
		if (!key_val_map.count(key)) return -1;
		updatePos(key);
		return key_val_map[key];
	}

	void put(int key, int value) {
        //check cap==0
        if(cap==0) return;
		//exist
		if (key_val_map.count(key)) {
			updatePos(key);
			key_val_map[key] = value;
			return;
		}
		//not exist
		//full - remove last
		if (key_val_map.size() == cap) {
			removeLast();
		}
		//add
        addNew(key, value);
	}

	void updatePos(int key) {
		//remove
		int freq = key_freq_map[key];
		freq_keyList_map[freq].erase(key_iter_map[key]);
		if (freq_keyList_map[min_freq].empty()) min_freq++;
		//add
		freq++;
		freq_keyList_map[freq].push_front(key);
		key_iter_map[key] = freq_keyList_map[freq].begin();
		key_freq_map[key]++;
	}

	void removeLast() {
		int key = freq_keyList_map[min_freq].back();
		freq_keyList_map[min_freq].pop_back();
		key_iter_map.erase(key);
		key_freq_map.erase(key);
		key_val_map.erase(key);
		//ignore min_freq
	}
    
    void addNew(int key, int value){
        freq_keyList_map[1].push_front(key);
		key_iter_map[key] = freq_keyList_map[1].begin();
		key_val_map[key] = value;
		key_freq_map[key] = 1;
		min_freq = 1;
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```