# Longest Palindrome by Concatenating Two Letter Words

#: 2131
Difficult: Medium
Tags: Array, Counting, Greedy, Hash, string
link: https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/description/
Priority: Medium
Status: HardToImplement, More Solution
Created time: December 24, 2023 12:45 AM
Last edited time: December 24, 2023 1:05 AM
source: googleFreq

You are given an array of strings `words`. Each element of `words` consists of **two** lowercase English letters.

Create the **longest possible palindrome** by selecting some elements from `words` and concatenating them in **any order**. Each element can be selected **at most once**.

Return *the **length** of the longest palindrome that you can create*. If it is impossible to create any palindrome, return `0`.

A **palindrome** is a string that reads the same forward and backward.

**Example 1:**

```
Input: words = ["lc","cl","gg"]
Output: 6
Explanation: One longest palindrome is "lc" + "gg" + "cl" = "lcggcl", of length 6.
Note that "clgglc" is another longest palindrome that can be created.

```

**Example 2:**

```
Input: words = ["ab","ty","yt","lc","cl","ab"]
Output: 8
Explanation: One longest palindrome is "ty" + "lc" + "cl" + "yt" = "tylcclyt", of length 8.
Note that "lcyttycl" is another longest palindrome that can be created.

```

**Example 3:**

```
Input: words = ["cc","ll","xx"]
Output: 2
Explanation: One longest palindrome is "cc", of length 2.
Note that "ll" is another longest palindrome that can be created, and so is "xx".

```

**Constraints:**

- `1 <= words.length <= 105`
- `words[i].length == 2`
- `words[i]` consists of lowercase English letters.

Why the hashset solution TLE?

Solution Editorial:

```cpp
class Solution {
public:
    int longestPalindrome(vector<string>& words) {
        unordered_map<string, int> count;
        for (const string& word : words) {
            count[word]++;
        }

        int answer = 0;
        bool central = false;

        for (const auto& [word, countOfTheWord] : count) {
            // if the word is a palindrome
            if (word[0] == word[1]) {
                if (countOfTheWord % 2 == 0) {
                    answer += countOfTheWord;
                } else {
                    answer += countOfTheWord - 1;
                    central = true;
                }
                // consider a pair of non-palindrome words such that one is the
                // reverse of another ({word[1], word[0]} is the reversed word)
            } else if (word[0] < word[1] && count.count({word[1], word[0]})) {
                answer += 2 * min(countOfTheWord, count[{word[1], word[0]}]);
            }
        }

        if (central) {
            answer++;
        }
        return 2 * answer;
    }
};

/*
for each
    if AB has BA, pull from set and res++
check center from XX
*/
```

Solution TLE:

```cpp
class Solution {
public:
    int longestPalindrome(vector<string>& words) {
        int res=0;
        multiset<string> st;
        for(auto& s: words){ st.insert(s); }

        bool checkCore=true;
        while(!st.empty()){
            string s=*st.begin();
            st.erase(st.begin());
            swap(s[0],s[1]);
            if(st.count(s)){
                res+=4;
                st.erase(st.find(s));
            }
            else if(s[0]==s[1] && checkCore==true){
                res+=2;
                checkCore=false;
            }
        }
        return res;
    }
};

/*
pick all XX, accumulate the len

for the other, put into multiSet
for each
    if AB has BA, pull from set and res++
*/
```