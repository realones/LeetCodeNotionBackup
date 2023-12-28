# Remove Duplicate Letters

#: 316
Difficult: Medium
Tags: Greedy, monotonic, stack, string
link: https://leetcode.com/problems/remove-duplicate-letters
Priority: High
Status: Dig More, No Idea At First
Created time: September 26, 2023 1:12 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is

**the smallest in lexicographical order**

among all possible results.

**Example 1:**

```
Input: s = "bcabc"
Output: "abc"

```

**Example 2:**

```
Input: s = "cbacdcbc"
Output: "acdb"

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

**Note:** This question is the same as 1081: [https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

comment:

```cpp
x x x c x x x c x x x c x x x
which c to use
if c must use, then later c should be skipped : visited[c]=1 => skip
if previous ones are bigger and still have freq>0, then remove previous ones and mark them as unvisited
```

Solution 1:

```cpp
/*
similiar to "Largest Rectangle under the histogram "

We need to keep the monotically decreasing substring that contains all the char in the s.
So we just use a vector to mimic the stack!

In fact this problem is also similiar to the problem that the maximum in the sliding windows, 
I strongly recommend you to grasp the sliding windows solutions.
*/
//单调递增栈
class Solution {
public:
    string removeDuplicateLetters(string s) {
        unordered_map<char,int> freq;//freq of each char
        for(auto ch : s)  freq[ch]++;
        
        unordered_set<char> visited;
        stack<char> stk; stk.push('0');
        /** the key idea is to keep a monotically increasing sequence **/
        for(auto c : s) {
            freq[c]--;
            /** to filter the previously visited elements **/
            if(visited.count(c))  continue;
            while(freq[stk.top()]>0 && stk.top()>c) {
                visited.erase(stk.top());
                stk.pop();
            }
            stk.push(c);
            visited.insert(c);
        }
        
        string res="";
        while(!stk.empty()){
            res=stk.top()+res; stk.pop();
        }
        return res.substr(1);
    }
};
```

Solution 2: same with above

```cpp
/*
similiar to "Largest Rectangle under the histogram "

We need to keep the monotically decreasing substring that contains all the char in the s.
So we just use a vector to mimic the stack!

In fact this problem is also similiar to the problem that the maximum in the sliding windows, 
I strongly recommend you to grasp the sliding windows solutions.
*/
//单调递增栈
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector<int> dict(256, 0);//freq of each char
        vector<bool> visited(256, false);
        for(auto ch : s)  dict[ch]++;
        string result = "0";
        /** the key idea is to keep a monotically increasing sequence **/
        for(auto c : s) {
            dict[c]--;
            /** to filter the previously visited elements **/
            if(visited[c])  continue;
            while(result.back()>c && dict[result.back()]) {
                visited[result.back()] = false;
                result.pop_back();
            }
            result += c;
            visited[c] = true;
        }
        return result.substr(1);
    }
};
```