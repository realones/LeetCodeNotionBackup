# Minimum Insertions to Balance a Parentheses String

#: 1541
Difficult: Medium
Tags: Greedy, stack, string
link: https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/description/
Priority: High
Status: HardToImplement, No Idea At First
Created time: December 8, 2023 1:30 PM
Last edited time: December 8, 2023 1:40 PM
source: facebookFreq

Given a parentheses string `s` containing only the characters `'('` and `')'`. A parentheses string is **balanced** if:

- Any left parenthesis `'('` must have a corresponding two consecutive right parenthesis `'))'`.
- Left parenthesis `'('` must go before the corresponding two consecutive right parenthesis `'))'`.

In other words, we treat `'('` as an opening parenthesis and `'))'` as a closing parenthesis.

- For example, `"())"`, `"())(())))"` and `"(())())))"` are balanced, `")()"`, `"()))"` and `"(()))"` are not balanced.

You can insert the characters `'('` and `')'` at any position of the string to balance it if needed.

Return *the minimum number of insertions* needed to make `s` balanced.

**Example 1:**

```
Input: s = "(()))"
Output: 1
Explanation: The second '(' has two matching '))', but the first '(' has only ')' matching. We need to add one more ')' at the end of the string to be "(())))" which is balanced.

```

**Example 2:**

```
Input: s = "())"
Output: 0
Explanation: The string is already balanced.

```

**Example 3:**

```
Input: s = "))())("
Output: 3
Explanation: Add '(' to match the first '))', Add '))' to match the last '('.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of `'('` and `')'` only.

Solution 1:

[https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/solutions/779928/simple-o-n-stack-solution-with-detailed-explanation/](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/solutions/779928/simple-o-n-stack-solution-with-detailed-explanation/)

```cpp
//cp from https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/solutions/779928/simple-o-n-stack-solution-with-detailed-explanation/
class Solution {
public:
    int minInsertions(string s) {
        int ans = 0;
        stack<int> t;
        for (char c : s) {
            if (c == '(') {
                if (t.empty() || t.top() == 2) t.push(2);
                else {
                    t.pop();
                    t.push(2);
                    ans++;
                }
            }
            else {
                if (t.empty()) {
                    t.push(1); ans++;
                } else if (t.top() == 1) {
                    t.pop();
                } else if (t.top() == 2) {
                    t.pop(); t.push(1);
                }
            }
        }
        while (!t.empty()) {
            ans += t.top();
            t.pop();
        }
        return ans;
    }
};
```

Solution wrong - ) is not consecutive

```cpp
class Solution {
public:
    int minInsertions(string s) {
        vector<char> stk;
        for(auto c: s){
          if(c=='('){stk.push_back(c);}
          else if(c==')'){
            stk.push_back(c);
            int n=stk.size();
            if(n>=3 && stk[n-1]==')' && stk[n-2]==')' && stk[n-3]=='('){
              stk.pop_back(); stk.pop_back(); stk.pop_back();
            }
          }
        }
        std::cout<< "stk" <<" -- "; for (const auto& elem : stk) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        int res=0;
        while(!stk.empty()){
          int n=stk.size();
          if(stk.back()==')'){
            //)) ()
            //)) ))) )() ()
            if(n>=2){
              res+=1;
              stk.pop_back();
              stk.pop_back();
            }
            else if(n==1){
              res+=2;
              stk.pop_back();
            }
          }
          else if(stk.back()=='('){
            //)( ((
            res+=2;
            stk.pop_back();
          }
        }
        return res;
    }
};
```