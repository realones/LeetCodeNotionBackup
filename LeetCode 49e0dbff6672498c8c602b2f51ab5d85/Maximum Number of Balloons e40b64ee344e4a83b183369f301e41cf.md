# Maximum Number of Balloons

#: 1189
Difficult: Easy
Tags: Counting, Hash, string
link: https://leetcode.com/problems/maximum-number-of-balloons/description/
Priority: Pass
Created time: December 10, 2023 9:10 PM
Last edited time: December 10, 2023 9:11 PM
source: microsoftFreq

Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.

You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG](https://assets.leetcode.com/uploads/2019/09/05/1536_ex1_upd.JPG)

```
Input: text = "nlaebolko"
Output: 1

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG](https://assets.leetcode.com/uploads/2019/09/05/1536_ex2_upd.JPG)

```
Input: text = "loonbalxballpoon"
Output: 2

```

**Example 3:**

```
Input: text = "leetcode"
Output: 0

```

**Constraints:**

- `1 <= text.length <= 104`
- `text` consists of lower case English letters only.

```cpp
class Solution {
public:
    int maxNumberOfBalloons(string text) {
        unordered_set<char> st({'b','a','l','o','n'});
        unordered_map<char,int> mp;
        for(auto c: text){
            if(st.count(c)) mp[c]++;
        }
        int cnt=INT_MAX;
        cnt=min(cnt, mp['b']);
        cnt=min(cnt, mp['a']);
        cnt=min(cnt, mp['l']/2);
        cnt=min(cnt, mp['o']/2);
        cnt=min(cnt, mp['n']);
        return cnt;
    }
};
```