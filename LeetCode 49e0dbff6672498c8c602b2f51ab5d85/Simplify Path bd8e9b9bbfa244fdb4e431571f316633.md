# Simplify Path

#: 71
Difficult: Medium
Tags: stack, string
link: https://leetcode.com/problems/simplify-path/
Priority: Low
Created time: June 4, 2023 1:34 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given a string `path`, which is an **absolute path** (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified **canonical path**.

In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (i.e. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.

The **canonical path** should have the following format:

- The path starts with a single slash `'/'`.
- Any two directories are separated by a single slash `'/'`.
- The path does not end with a trailing `'/'`.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`)

Return *the simplified **canonical path***.

**Example 1:**

```
Input: path = "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.

```

**Example 2:**

```
Input: path = "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.

```

**Example 3:**

```
Input: path = "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.

```

**Constraints:**

- `1 <= path.length <= 3000`
- `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
- `path` is a valid absolute Unix path.

```cpp
class Solution {
public:
    string simplifyPath(string s) {
        string res;
        stack<string> stk;
        for(int l=0,rr=0;l!=string::npos && rr!=s.size();){
            l=s.find_first_not_of("/",rr); if(l==string::npos) break;
            rr=s.find_first_of("/",l); if(rr==string::npos) rr=s.size();
            string cur=s.substr(l, rr-l);
            if(cur=="" or cur=="."){continue;}
            else if(cur==".."){if(!stk.empty()) stk.pop();}
            else{stk.push(cur);}
        }
        
        while(!stk.empty()){
            string cur=stk.top(); stk.pop();
            res="/"+cur+res;
        }
        if(res=="") return "/";
        return res;
    }
};
```