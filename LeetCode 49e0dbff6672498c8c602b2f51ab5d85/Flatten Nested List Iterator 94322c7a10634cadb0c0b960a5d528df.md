# Flatten Nested List Iterator

#: 341
Difficult: Medium
Tags: iterator, stack
link: https://leetcode.com/problems/flatten-nested-list-iterator/
Priority: Low
Created time: September 25, 2022 4:52 PM
Last edited time: October 31, 2023 2:56 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the `NestedIterator` class:

- `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list `nestedList`.
- `int next()` Returns the next integer in the nested list.
- `boolean hasNext()` Returns `true` if there are still some integers in the nested list and `false` otherwise.

Your code will be tested with the following pseudocode:

```
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res

```

If `res` matches the expected flattened list, then your code will be judged as correct.

**Example 1:**

```
Input: nestedList = [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].

```

**Example 2:**

```
Input: nestedList = [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].

```

**Constraints:**

- `1 <= nestedList.length <= 500`
- The values of the integers in the nested list is in the range `[-106, 106]`.

Iterator

1. List 转 Stack
2. 主函数逻辑放在HasNext里面
3. Next只做一次pop处理

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    stack<NestedInteger> stk;
    
    NestedIterator(vector<NestedInteger> &nestedList) {
        pushVec(nestedList);
    }
    
    int next() {
        if(hasNext()){
            int res=stk.top().getInteger(); stk.pop();
            return res;
        }
        return INT_MIN;
    }
    
    bool hasNext() {
        while(!stk.empty() && !stk.top().isInteger()){
            NestedInteger cur = stk.top(); stk.pop();
            pushVec(cur.getList());
        }
        return !stk.empty();
    }
    
    void pushVec(vector<NestedInteger> &nestedList){
        int n=nestedList.size(); if(n==0) {return;}
        for(int i=n-1;i>=0;i--){
            stk.push(nestedList[i]);
        }
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```