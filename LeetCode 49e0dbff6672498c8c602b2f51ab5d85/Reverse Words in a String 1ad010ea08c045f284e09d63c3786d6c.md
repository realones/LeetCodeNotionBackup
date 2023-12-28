# Reverse Words in a String

#: 151
Difficult: Medium
Tags: 2pt, 3 Step Rotate, string
link: https://leetcode.com/problems/reverse-words-in-a-string/
Priority: High
Status: More Solution
Created time: May 31, 2023 3:47 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, LC_Top_Interview_150

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"

```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.

```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

solution1:

```cpp
class Solution {
public:
    string reverseWords(string s) {
        trimString(s);
        for(int l=0,rr=0;l!=string::npos && rr!=s.size();){
            l=s.find_first_not_of(" ",rr); if(l==string::npos) break;
            rr=s.find_first_of(" ",l); if(rr==string::npos) rr=s.size();
            reverse(s.begin()+l, s.begin()+rr);
        }
        reverse(s.begin(),s.end());
        return s;
    }
    
    void trimString(string& s) {
        istringstream iss(s);
        ostringstream oss;
        copy(istream_iterator<string>(iss >> std::ws), istream_iterator<string>(), ostream_iterator<string>(oss, " "));
        string trimmed = oss.str();
        trimmed.erase(trimmed.find_last_not_of(" ") + 1);
        s = trimmed;
    }
};
```

solution2:

```cpp
class Solution {
public:
    void reverseWords(string &s) {
    reverse(s.begin(), s.end());
    int storeIndex = 0;
    for (int i = 0; i < s.size(); i++) {
        if (s[i] != ' ') {
            if (storeIndex != 0) s[storeIndex++] = ' ';
            int j = i;
            while (j < s.size() && s[j] != ' ') { s[storeIndex++] = s[j++]; }
            reverse(s.begin() + storeIndex - (j - i), s.begin() + storeIndex);
            i = j;
        }
    }
    s.erase(s.begin() + storeIndex, s.end());
}
};
```

solution 3:

```cpp
class Solution {
public:
    void reverseWords(string &s) {
        trim(s);
        reverse(s.begin(),s.end());
        int start=0,end=0;
        while(start<s.size()){
            start=s.find_first_not_of(' ',start);
            if(start==-1) return;
            end=s.find_first_of(' ',start);
            if(end==-1){end=s.size()-1;}
            else end--;
            reverse(s.begin()+start,s.begin()+end+1);
            start=end+1;
        }
    }
    
    void trim(string& str){
    // remove trailing white space
    while(!str.empty() && isspace( str.back() )) str.pop_back() ;

    // return residue after leading white space
    int pos = 0 ;
    while(pos < str.size() && std::isspace( str[pos] )) ++pos ;
    str=str.substr(pos);
        
    // trim middle space
    for(int i=1;i<str.size();i++){
        if(str[i]==' ' && str[i-1]==' '){str.erase(i,1);i--;}
    }
}
};
```