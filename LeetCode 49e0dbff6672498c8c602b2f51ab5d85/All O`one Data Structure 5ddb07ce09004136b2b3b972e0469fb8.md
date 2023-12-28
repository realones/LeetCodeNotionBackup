# All O`one Data Structure

#: 432
Difficult: Hard
Tags: Design, Hash, LinkedList
link: https://leetcode.com/problems/all-oone-data-structure/description/
Priority: High
Status: HardToImplement, checkBetterSolution, toRevisit, toSummarize
Created time: June 27, 2023 9:28 PM
Last edited time: October 21, 2023 1:09 PM
source: HuaHua
related: LFU Cache (LFU%20Cache%20653efcca4f3848b7b71f50e37e74a8d8.md)

Design a data structure to store the strings' count with the ability to return the strings with minimum and maximum counts.

Implement the `AllOne` class:

- `AllOne()` Initializes the object of the data structure.
- `inc(String key)` Increments the count of the string `key` by `1`. If `key` does not exist in the data structure, insert it with count `1`.
- `dec(String key)` Decrements the count of the string `key` by `1`. If the count of `key` is `0` after the decrement, remove it from the data structure. It is guaranteed that `key` exists in the data structure before the decrement.
- `getMaxKey()` Returns one of the keys with the maximal count. If no element exists, return an empty string `""`.
- `getMinKey()` Returns one of the keys with the minimum count. If no element exists, return an empty string `""`.

**Note** that each function must run in `O(1)` average time complexity.

**Example 1:**

```
Input
["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]
Output
[null, null, null, "hello", "hello", null, "hello", "leet"]

Explanation
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "hello"
allOne.inc("leet");
allOne.getMaxKey(); // return "hello"
allOne.getMinKey(); // return "leet"

```

**Constraints:**

- `1 <= key.length <= 10`
- `key` consists of lowercase English letters.
- It is guaranteed that for each call to `dec`, `key` is existing in the data structure.
- At most `5 * 104` calls will be made to `inc`, `dec`, `getMaxKey`, and `getMinKey`.

todo: check solution: [https://leetcode.com/problems/all-oone-data-structure/solutions/91398/c-solution-with-comments/](https://leetcode.com/problems/all-oone-data-structure/solutions/91398/c-solution-with-comments/)

Solution:

ver1:

```cpp
class AllOne {
public:
    //list: (2,strList) - (4,strList) - (5,strList) - (8,strList)
    list<pair<int,list<string>>> freq_keys_list;//front is smaller freq
    unordered_map<int,list<pair<int,list<string>>>::iterator> freq_listIter; //freq_listIter
    unordered_map<string, list<string>::iterator> key_iter;
    unordered_map<string,int> key_freq;

    AllOne() { }
    
    void inc(string key) {
        //not exist
        if(!key_iter.count(key)){  //add to freq==1
            if(freq_keys_list.front().first==1){
                freq_keys_list.front().second.push_front(key);
            }
            else{
                freq_keys_list.emplace_front(1, list<string>{key});
            }
            freq_listIter[1]=freq_keys_list.begin();
            key_iter[key]=freq_keys_list.front().second.begin();
            key_freq[key]=1;
        }
        else{ //exist
            //freq++
            update(key,true);
        }
    }
    
    void dec(string key) {
        //freq--, if new f==0 then delete
        int f=key_freq[key];
        if(f-1==0){
            erase(key);
            return;
        }
        update(key,false);
    }
    
    string getMaxKey() {
        return freq_keys_list.back().second.front();
    }
    
    string getMinKey() {
        return freq_keys_list.front().second.front();
    }

    //if inc, then inc; otherwise dec
    //allow freq=0 after update
    void update(string& key, bool inc){
        //find
        int f=key_freq[key];
        auto listIter=freq_listIter[f];
        //update
        if(inc){
            //if nextF == f+1, add to next(listIter), else create new list
            //create new node
            if(next(listIter)!=freq_keys_list.end() && next(listIter)->first==f+1)
                next(listIter)->second.push_front(key);//lru - push latest in front
            else
                freq_keys_list.insert(next(listIter), {f+1, {key}});
            //erase old node
            listIter=next(listIter);
            erase(key);
            //update info
            freq_listIter[f+1]=listIter;
            key_iter[key]=listIter->second.begin();
            key_freq[key]=f+1;
        }
        else{//dec
            //if prevF == f-1, add to prev(listIter), else create new list
            //create new node
            if(listIter!=freq_keys_list.begin() && prev(listIter)->first==f-1)
                prev(listIter)->second.push_front(key);//lru - push latest in front
            else
                freq_keys_list.insert(listIter, {f-1, {key}});
            //erase old node
            listIter=prev(listIter);
            erase(key);
            //update info
            freq_listIter[f-1]=listIter;
            key_iter[key]=listIter->second.begin();
            key_freq[key]=f-1;
        }
    }

    //return the iterator pointing to the new location of the element that followed the last element erased by the function call. 
    list<pair<int,list<string>>>::iterator erase(string& key){
        //find
        int f=key_freq[key];
        auto listIter=freq_listIter[f];
        //delete
        listIter->second.erase(key_iter[key]);
        if(listIter->second.empty()) {
            listIter=freq_keys_list.erase(listIter);
            freq_listIter.erase(f);
        }
        key_iter.erase(key);
        key_freq.erase(key);
        return listIter;
    }
};

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne* obj = new AllOne();
 * obj->inc(key);
 * obj->dec(key);
 * string param_3 = obj->getMaxKey();
 * string param_4 = obj->getMinKey();
 */
```

ver2:

```cpp
class AllOne {
public:
    //list: (2,strList) - (4,strList) - (5,strList) - (8,strList)
    list<pair<int,list<string>>> freq_keys_list;//front is smaller freq
    unordered_map<string,list<pair<int,list<string>>>::iterator> key_listIter;
    unordered_map<string, list<string>::iterator> key_iter;//for lru cache, otherwise use st cache in above

    AllOne() { }
    
    void inc(string key) {
        //not exist
        if(!key_listIter.count(key)){  //add to freq==1
            if(freq_keys_list.front().first==1){
                freq_keys_list.front().second.push_front(key);
            }
            else{
                freq_keys_list.push_front({1, {key}});
            }
            key_listIter[key]=freq_keys_list.begin();
            key_iter[key]=freq_keys_list.front().second.begin();
        }
        else{ //exist
            //freq++
            update(key,true);
        }
    }
    
    void dec(string key) {
        //freq--, if new f==0 then delete
        int f=key_listIter[key]->first;
        if(f-1==0){
            erase(key);
        }
        else
            update(key,false);
    }
    
    string getMaxKey() {
        return freq_keys_list.back().second.front();
    }
    
    string getMinKey() {
        return freq_keys_list.front().second.front();
    }

    //if inc, then inc; otherwise dec
    //allow freq=0 after update
    void update(string& key, bool inc){
        //find
        auto listIter=key_listIter[key];
        int f=listIter->first;
        //update
        if(inc){
            //if nextF == f+1, add to next(listIter), else create new list
            //create new node
            if(next(listIter)!=freq_keys_list.end() && next(listIter)->first==f+1)
                next(listIter)->second.push_front(key);//lru - push latest in front
            else
                freq_keys_list.insert(next(listIter), {f+1, {key}});
            //erase old node
            listIter=next(listIter);
            erase(key);
            //update info
            key_listIter[key]=listIter;
            key_iter[key]=listIter->second.begin();
        }
        else{//dec
            //if prevF == f-1, add to prev(listIter), else create new list
            //create new node
            if(listIter!=freq_keys_list.begin() && prev(listIter)->first==f-1)
                prev(listIter)->second.push_front(key);//lru - push latest in front
            else
                freq_keys_list.insert(listIter, {f-1, {key}});
            //erase old node
            listIter=prev(listIter);
            erase(key);
            //update info
            key_listIter[key]=listIter;
            key_iter[key]=listIter->second.begin();
        }
    }

    //return the iterator pointing to the new location of the element that followed the last element erased by the function call. 
    list<pair<int,list<string>>>::iterator erase(string& key){
        //find
        auto listIter=key_listIter[key];
        //delete from chain
        listIter->second.erase(key_iter[key]);
        if(listIter->second.empty())
            listIter=freq_keys_list.erase(listIter);
        //delete iter info
        key_listIter.erase(key);
        key_iter.erase(key);
        return listIter;
    }
};
```

ver3: todo

```cpp

```