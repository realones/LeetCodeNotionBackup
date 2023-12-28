# Find the Closest Palindrome

#: 564
Difficult: Hard
Tags: Math, string
link: https://leetcode.com/problems/find-the-closest-palindrome/description/
Priority: Medium
Status: checkBetterSolution
Created time: December 18, 2023 9:58 PM
Last edited time: December 18, 2023 9:58 PM
source: googleFreq

Given a string `n` representing an integer, return *the closest integer (not including itself), which is a palindrome*. If there is a tie, return ***the smaller one***.

The closest is defined as the absolute difference minimized between two integers.

**Example 1:**

```
Input: n = "123"
Output: "121"

```

**Example 2:**

```
Input: n = "1"
Output: "0"
Explanation: 0 and 2 are the closest palindromes but we return the smallest which is 0.

```

**Constraints:**

- `1 <= n.length <= 18`
- `n` consists of only digits.
- `n` does not have leading zeros.
- `n` is representing an integer in the range `[1, 1018 - 1]`.

```cpp
using ll=long long;
class Solution {
public:
    string nearestPalindromic(string s) {
        int n=s.size();

        auto trans = [&](string s){
            int n=s.size();
            string mid="";
            if(n%2==1) mid=s[n/2];
            string left=s.substr(0,n/2);
            string right=left;
            reverse(right.begin(), right.end());
            return stol(left+mid+right);
        };

        ll x=stol(s);
        int p=pow(10,(n/2));
        string s1=to_string(x-p);
        if(s1.size() < s.size()){
            for(int i=s1.size()/2;i<s1.size();i++){
                s1[i]='9';
            }
        }
        string s2=s;
        string s3=to_string(x+p);
        if(s3.size() > s.size()){
            for(int i=(s3.size()+1)/2;i<s3.size();i++){
                s3[i]='0';
            }
        }

        ll can1=trans(s1);
        ll can2=trans(s2); if(can2==x) can2=INT_MIN/2;
        ll can3=trans(s3);

        ll res=-1;
        ll maxDiff=LLONG_MAX;
        if(abs(x-can1)<maxDiff) res=can1, maxDiff=abs(x-can1);
        if(abs(x-can2)<maxDiff) res=can2, maxDiff=abs(x-can2);
        if(abs(x-can3)<maxDiff) res=can3, maxDiff=abs(x-can3);
        return to_string(res);
    }
};
```