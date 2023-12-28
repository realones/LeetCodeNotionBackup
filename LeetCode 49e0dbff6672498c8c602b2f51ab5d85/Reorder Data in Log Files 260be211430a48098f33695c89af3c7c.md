# Reorder Data in Log Files

#: 937
Difficult: Medium
Tags: Array, Sort, string
link: https://leetcode.com/problems/reorder-data-in-log-files/
Priority: Low
Created time: November 16, 2023 1:36 AM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You are given an array of `logs`. Each log is a space-delimited string of words, where the first word is the **identifier**.

There are two types of logs:

- **Letter-logs**: All words (except the identifier) consist of lowercase English letters.
- **Digit-logs**: All words (except the identifier) consist of digits.

Reorder these logs so that:

1. The **letter-logs** come before all **digit-logs**.
2. The **letter-logs** are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
3. The **digit-logs** maintain their relative ordering.

Return *the final order of the logs*.

**Example 1:**

```
Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
Explanation:
The letter-log contents are all different, so their ordering is "art can", "art zero", "own kit dig".
The digit-logs have a relative order of "dig1 8 1 5 1", "dig2 3 6".

```

**Example 2:**

```
Input: logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
Output: ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]

```

**Constraints:**

- `1 <= logs.length <= 100`
- `3 <= logs[i].length <= 100`
- All the tokens of `logs[i]` are separated by a **single** space.
- `logs[i]` is guaranteed to have an identifier and at least one word after the identifier.

```cpp
class Solution {
public:
    vector<string> reorderLogFiles(vector<string>& logs) {
        vector<pair<string,string>> v1;//for letter log, con_id for sorting purpose
        vector<string> v2;//for digit log
        for(auto& l: logs){
            if(isLetter(l))
                v1.push_back(getContentId(l));
            else
                v2.push_back(l);
        }
        sort(v1.begin(), v1.end());

        vector<string> res;
        for(auto& [con,id]: v1){
            res.push_back(buildLog(id,con));
        }
        for(auto& l: v2){
            res.push_back(l);
        }
        return res;
    }

    bool isLetter(string& l){
        int pos=l.find(" ");
        return isalpha(l[pos+1]);
    }

    pair<string,string> getContentId(string& l){
        int pos=l.find(" ");
        string id=l.substr(0,pos);
        string con=l.substr(pos+1);
        return {con,id};
    }

    string buildLog(string& id, string& con){
        return id+" "+con;
    }
};

/*
for content is letter:
pair<content,id> - sort - vector/set

for content is digit
origin order - vector
*/
```