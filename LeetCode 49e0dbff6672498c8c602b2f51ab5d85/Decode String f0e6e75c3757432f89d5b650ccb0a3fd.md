# Decode String

#: 394
Difficult: Medium
Tags: recursion, stack
link: https://leetcode.com/problems/decode-string/
Priority: High
Status: More Solution
Created time: September 5, 2022 11:11 PM
Last edited time: November 23, 2023 2:42 AM
practicedTimes: 1
source: HuaHua, LC_75, jiuzhang, 算高Heap&Stack

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed `105`.

**Example 1:**

```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"

```

**Example 2:**

```
Input: s = "3[a2[c]]"
Output: "accaccacc"

```

**Example 3:**

```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"

```

**Constraints:**

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
- `s` is guaranteed to be **a valid** input.
- All the integers in `s` are in the range `[1, 300]`.

solutions 

```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<string> stk;
        for(auto c:s){
            if(isdigit(c)){
                if(!stk.empty()&&isdigit(stk.top()[0])){
                    stk.top()=to_string(stoi(stk.top())*10+(c-'0'));
                }
                else stk.push(string(1,c));
            }
            else if(isalpha(c)){
                if(!stk.empty()&&isalpha(stk.top()[0])){
                    stk.top()+=string(1,c);
                }
                else stk.push(string(1,c));
            }
            else if(c=='['){stk.push("[");}
            else if(c==']'){
                string str=stk.top(); stk.pop();
                stk.pop(); //pop [
                int cnt=stoi(stk.top()); stk.pop();
                string newStr="";
                for(int i=0; i<cnt; i++){ newStr+=str; }
                if(!stk.empty()&&isalpha(stk.top()[0])){
                    stk.top()+=newStr;
                }
                else stk.push(newStr);
            }
        }
        return stk.top();
    }
};

/*
31[abc2[de]f]g
31: when num, if top is num, ++, else in stk new top
 [: in stk
  abc: when char, if top is char, ++, else in stk new top
     2: in stk
      [: in stk
       de in stk
         ]: de ], de str, de[, de cnt - there must be one str till [
          f
return top
*/
```

stack - string

```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<string> stk;
        while(!s.empty()){
            if(isalpha(s[0])){
                string alphas=popAlpha(s);
                pushAlpha(stk,alphas);
            }
            else if(isdigit(s[0])){
                string cntStr=popNum(s);
                stk.push(cntStr);
            }
            else if('['==s[0]){s=s.substr(1);}
            else if(']'==s[0]){
                updateStk(stk);
                s=s.substr(1);
            }
        }
        return stk.top();
    }
    //popNum
    string popNum(string &s){
        int i=s.find('[');
        string top=s.substr(0,i);
        s=s.substr(i);
        return top;
    }
    //popAlphabetics
    string popAlpha(string &s){
        int i=0;
        while(i<s.size() && isalpha(s[i])) i++;
        string top=s.substr(0,i);
        s=s.substr(i);
        return top;
    }
    //updateStk
    void updateStk(stack<string> &stk){
        string alphas=stk.top(); stk.pop();
        string cntStr=stk.top(); stk.pop();
        int cnt=stoi(cntStr);
        string newAlphas="";
        for(int i=0; i<cnt; i++){
            newAlphas+=alphas;
        }
        pushAlpha(stk,newAlphas);
    }
    //popAlphas
    void pushAlpha(stack<string> &stk, string s){
        if(!stk.empty() && isalpha(stk.top()[0])){
            string tmp = stk.top(); stk.pop();
            s=tmp+s;
        }
        stk.push(s);
    }
};
```

stack - char , int

todo: use one stk

```cpp
class Solution {
public:
    string decodeString(string s) {
        //number stack : need to combine number
        //char stack: contains [ as bot
        //see ]: pop num, char until [, push back to char stack
        stack<int> numStk;
        stack<char> charStk;
        
        int num=0;
        for(auto c:s){
            if(isdigit(c)){
                num=num*10+c-'0';
            }
            else if(isalpha(c)){
                charStk.push(c);
            }
            else if(c=='['){
                numStk.push(num);
                charStk.push('[');
                num=0;
            }
            else{//]
                int cnt=numStk.top();numStk.pop();
                string tmp="";
                while(charStk.top()!='['){
                    tmp=charStk.top()+tmp;charStk.pop();
                }
                charStk.pop();//pop [
                string newTmp="";
                for(int i=0;i<cnt;i++){newTmp+=tmp;}
                for(auto ch:newTmp){
                    charStk.push(ch);
                }
            }
        }
        string res="";
        while(!charStk.empty()){
            res=charStk.top()+res;charStk.pop();
        }
        return res;
    }
};
```

recursive: todo

```cpp
class Solution {
public:
    // aaa ddd[??] aaa 
    // aaa or dddd[??] is a function
    string decodeString(const string& s, int& i) {
        string res;
        
        while (i < s.length() && s[i] != ']') {
            if (isalpha(s[i]))//alpha
                res += s[i++];
            else {//digit
                int cnt = 0;
                while (i < s.length() && isdigit(s[i]))
                    cnt = cnt * 10 + s[i++] - '0';
                    
                i++; // '['
                string t = decodeString(s, i);
                i++; // ']'
                
                while (cnt-- > 0)
                    res += t;
            }
        }
        
        return res;
    }

    string decodeString(string s) {
        int i=0;
        return decodeString(s, i);
    }
};
```