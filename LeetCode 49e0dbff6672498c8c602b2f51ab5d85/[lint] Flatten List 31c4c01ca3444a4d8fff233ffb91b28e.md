# [lint] Flatten List

#: 22
Difficult: Easy
Tags: stack
link: https://www.lintcode.com/problem/22/
Priority: Pass
Created time: September 25, 2022 4:33 PM
Last edited time: October 31, 2023 2:58 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Description

Given a list, each element in the list can be a list or an integer.Flatten it into a simply list with integers.

If the element in the given list is a list, it can contain list too.

Example

**Example 1:**

Input:

```
list = [[1,1],2,[1,1]]

```

Output:

```
[1,1,2,1,1]

```

Explanation:

Flatten it into a simply list with integers.

**Example 2:**

Input:

```
list = [1,2,[1,2]]

```

Output:

```
[1,2,1,2]

```

Explanation:

Flatten it into a simply list with integers.

**Example 3:**

Input:

```
list = [4,[3,[2,[1]]]]

```

Output:

```
[4,3,2,1]

```

Explanation:

Flatten it into a simply list with integers.

Challenge

Do it in non-recursive.

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer,
 *     // rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds,
 *     // if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds,
 *     // if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    // @param nestedList a list of NestedInteger
    // @return a list of integer
    vector<int> flatten(vector<NestedInteger> &nestedList) {
        // Write your code here
        int n=nestedList.size(); if(n==0) {return {};}
        
        vector<int> res;
        stack<NestedInteger> stk;
        for(int i=n-1;i>=0;i--){
            stk.push(nestedList[i]);
        }

        while(!stk.empty()){
            NestedInteger top=stk.top(); stk.pop();
            if(top.isInteger()) {res.push_back(top.getInteger());}
            else{
                vector<NestedInteger> list=top.getList();
                for(int i=list.size()-1;i>=0;i--){
                    stk.push(list[i]);
                }
            }
        }
        
        
        return res;
    }
};
```