# Remove Invalid Parentheses

#: 301
Difficult: Hard
Tags: BFS, BackTracking, string
link: https://leetcode.com/problems/remove-invalid-parentheses/description/
Priority: High
Status: More Solution, No Idea At First
Created time: June 28, 2023 12:00 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a string `s` that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return *a list of **unique strings** that are valid with the minimum number of removals*. You may return the answer in **any order**.

**Example 1:**

```
Input: s = "()())()"
Output: ["(())()","()()()"]

```

**Example 2:**

```
Input: s = "(a)())()"
Output: ["(a())()","(a)()()"]

```

**Example 3:**

```
Input: s = ")("
Output: [""]

```

**Constraints:**

- `1 <= s.length <= 25`
- `s` consists of lowercase English letters and parentheses `'('` and `')'`.
- There will be at most `20` parentheses in `s`.

Solutions:

solution BFS:

```cpp
//if find all solution, need to traverse all possibilities
//bfs or dfs, since find min, use bfs
//two loops in bfs to stop loop at min

class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        if(s=="") return vector<string>{""};
        vector<string> res;
        unordered_set<string> visited;
        
        bool endLoop=false;
        queue<string> q;
        q.push(s); visited.insert(s);
        while(!q.empty()){
            int sz=q.size();
            for(int i=0;i<sz;i++){
                string cur=q.front(); q.pop();
                //check valid
                if(isValid(cur)){
                    endLoop=true;
                    res.push_back(cur);
                }
                //delete kth char from cur
                if(endLoop) continue;
                for(int k=0;k<cur.size();k++){
                    string newStr=cur.substr(0,k)+cur.substr(k+1);
                    if(visited.count(newStr)) continue;
                    visited.insert(newStr);
                    q.push(newStr);
                }
            }
            if(endLoop) break;
        }
        
        return res;
    }
    
    bool isValid(string s){
        if(s=="") return true;
        int cnt=0;
        //simulate stack, not need stack since just one kind of paratheses
        for(auto c:s){
            if(c=='(') cnt++;
            else if(c==')') {
                if(cnt==0) return false;
                cnt--;
            }
        }
        return cnt==0;
    }
};
```

Solution DFS: to be improved with backtracking

```cpp
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        int remove_left=0, remove_right=0, ldr=0;
        /*** use the unordered_set to deal with the duplicate cases ***/
        unordered_set<string> result;
        /***  calculate the remained # of left and right parentheses  ***/
        for(int i=0; i<s.size(); i++){
            if(s[i]=='(')   remove_left++;
            else if(s[i]==')'){
                if(remove_left > 0) remove_left--;
                else    remove_right++;
            }
        }
        help(0, 0, remove_left, remove_right, s, "", result);
        /*** change the unordered_set to vector ***/
        return vector<string>(result.begin(), result.end());
    }

    /***
    ldr : current cnt of leftParentheses - rightleftParentheses
    index : record the cur-position int the string s
    remove_left : the number of left parentheses needed to delete
    remove_right : the number of right parentheses needed to delete
    s : origninal input string    solution : the current produced string
    result : stores all the satisfied solution string
    ***/
    void help(int ldr, int index, int remove_left, int remove_right, const string& s, string solution, unordered_set<string>& result){
        /***   end condition       ***/
        if(index==s.size()){
            /*** check whether the remained string solution is right  ***/
            if(ldr==0 && remove_left==0 && remove_right==0)    result.insert(solution);
            return;
        }
        /***    left-half-parentheses     ***/
        if(s[index]=='('){
            /***    remove the left-half-parentheses     ***/
            if(remove_left > 0)     help(ldr, index+1, remove_left-1, remove_right, s, solution, result);
            /***    keep  the  left-half-parentheses     ***/
            help(ldr+1, index+1, remove_left, remove_right, s, solution+s[index], result);
        }
        /***    right-half-parentheses     ***/
        else if(s[index]==')'){
            /***    remove the right-half-parentheses     ***/
            if(remove_right > 0)     help(ldr, index+1, remove_left, remove_right-1, s, solution, result);
            /***    keep  the  right-half-parentheses     ***/
            if(ldr > 0) help(ldr-1, index+1, remove_left, remove_right, s, solution+s[index], result);
        }
        /***    other-characters     ***/
        else{
            help(ldr, index+1, remove_left, remove_right, s, solution+s[index], result);
        }
    }
};
```