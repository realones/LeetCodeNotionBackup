# Number of Atoms

#: 726
Difficult: Hard
Tags: Hash, OrderMap, Sort, recursion, stack, string
link: https://leetcode.com/problems/number-of-atoms/
Priority: High
Status: HardToImplement, checkBetterSolution, toRevisit, toSummarize
Created time: June 20, 2023 8:57 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua

Given a string `formula` representing a chemical formula, return *the count of each atom*.

The atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

One or more digits representing that element's count may follow if the count is greater than `1`. If the count is `1`, no digits will follow.

- For example, `"H2O"` and `"H2O2"` are possible, but `"H1O2"` is impossible.

Two formulas are concatenated together to produce another formula.

- For example, `"H2O2He3Mg4"` is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula.

- For example, `"(H2O2)"` and `"(H2O2)3"` are formulas.

Return the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than `1`), followed by the second name (in sorted order), followed by its count (if that count is more than `1`), and so on.

The test cases are generated so that all the values in the output fit in a **32-bit** integer.

**Example 1:**

```
Input: formula = "H2O"
Output: "H2O"
Explanation: The count of elements are {'H': 2, 'O': 1}.

```

**Example 2:**

```
Input: formula = "Mg(OH)2"
Output: "H2MgO2"
Explanation: The count of elements are {'H': 2, 'Mg': 1, 'O': 2}.

```

**Example 3:**

```
Input: formula = "K4(ON(SO3)2)2"
Output: "K4N2O14S4"
Explanation: The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.

```

**Constraints:**

- `1 <= formula.length <= 1000`
- `formula` consists of English letters, digits, `'('`, and `')'`.
- `formula` is always valid.

**Solution**:
Implementation takes a lot of time, ~2hours, and complex, check online for better implementation

to summarize the
for(int l=0,r=0; l<n && r<n;l=r+1, r=l)
while(r+1<n && (islower(formula[r+1]))) r++;

```cpp
class Solution {
public:
    string countOfAtoms(string formula) {
        /*
        K4(OgN(SO3))2
        ==>
        K4(Og1N1(S1O3)1)2
        */
        for(int i=0; i<formula.size(); i++){
            if(isalpha(formula[i])){
                if(i+1==formula.size() || !(islower(formula[i+1]) || isdigit(formula[i+1]))){
                    formula.insert(i+1, "1");
                }
            }
        }
        for(int i=0; i<formula.size(); i++){
            if(')' == formula[i]){
                if(i+1==formula.size() || !isdigit(formula[i+1])){
                    formula.insert(i+1, "1");
                }
            }
        }
        std::cout << "formula" << " -- " << formula << std::endl;
        
        //define section e.g. Og12
        int n=formula.size();
        stack<pair<string,int>> stk;
        for(int l=0,r=0; l<n && r<n;l=r+1, r=l){
            if(formula[l]=='('){ // "("
                stk.push({"(",1});
                std::cout << "(" << std::endl;
            }
            else if(formula[l]==')'){ //")cnt"
                while(r+1<n && isdigit(formula[r+1])) r++;
                string s=formula.substr(l,r-l+1);
                std::cout << ")cnt" << " -- " << s << std::endl;
                int cnt=stoi(s.substr(1));

                vector<pair<string,int>> secs;
                while(stk.top().first != "("){
                    secs.push_back(stk.top()); stk.pop();
                }
                stk.pop(); //pop (
                for(auto& sec: secs){
                    sec.second *= cnt;
                    stk.push(sec);
                }
            }
            else if(isupper(formula[l])){ //"section"
                while(r+1<n && (islower(formula[r+1]))) r++;
                string atom=formula.substr(l,r-l+1);
                l=r+1,r=l;
                while(r+1<n && (isdigit(formula[r+1]))) r++;
                string cntStr=formula.substr(l,r-l+1);
                int cnt=stoi(cntStr);
                stk.push({atom, cnt});
                std::cout << "section" << " -- " << atom << "-" << cnt << std::endl;
            }
            else{std::cout << "error" << " -- " << formula[l] << std::endl;}
        }

        std::cout<< "stk" <<" -- "; auto tempCon = stk; vector<pair<string,int>> temp(tempCon.size()); for (auto it = temp.rbegin(); !tempCon.empty(); ++it, tempCon.pop()) *it = tempCon.top(); for (const auto &str : temp) cout << str.first <<"-"<<str.second << " "; std::cout<<std::endl;

        map<string,int> mp;
        while(!stk.empty()){
            mp[stk.top().first]+= stk.top().second; stk.pop();
        }

        string res="";
        for(auto [atom, cnt]: mp){
            res += atom + (cnt==1?"":to_string(cnt));
        }
        return res;
        //put into TreeMap and sort and massage result
    }
};

/*
RULE:
piece = 
atomic:
    1U + [0..n]L
    Z Ab Bcg
+
digits
    [0..n] digitChar
    "" "3" "22"
    "" means 1
    "1" not allowed

pieces an be like:
    (piece)
    (piece) piece
    (piece)digits

map[atomic]=count
return atom+cnt(if>1) + atom+cnt(if>1) +...

IMPL:
K4(OgN(SO3)2)2
==>
K4(Og1N1(S1O3)2)2

define section e.g. Og12

for each part in formula
    if (
        push to stk
    if )cnt
        pop section
        pop (
        push section*cnt
    if section
        push to stk

put into TreeMap and sort and massage result
*/
```