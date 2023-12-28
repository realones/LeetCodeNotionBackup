# Accounts Merge

#: 721
Difficult: Medium
Tags: Array, BFS, DFS, Graph, Union Find, string
link: https://leetcode.com/problems/accounts-merge/description/
Priority: Medium
Status: HardToImplement
Created time: August 24, 2023 4:17 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a list of `accounts` where each element `accounts[i]` is a list of strings, where the first element `accounts[i][0]` is a name, and the rest of the elements are **emails** representing emails of the account.

Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.

After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails **in sorted order**. The accounts themselves can be returned in **any order**.

**Example 1:**

```
Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
Explanation:
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'],
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.

```

**Example 2:**

```
Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]

```

**Constraints:**

- `1 <= accounts.length <= 1000`
- `2 <= accounts[i].length <= 10`
- `1 <= accounts[i][j].length <= 30`
- `accounts[i][0]` consists of English letters.
- `accounts[i][j] (for j > 0)` is a valid email.

comment: 

easy to figure out using unionFind, but need tricks to handle name (how to guarantee the name is always the root of a group, but not using email)

```cpp
class Solution {
public:
    unordered_map<string,string> father;

    //with pass compression
    // O(1)
    string find(string a){
        if(!father.count(a)) father[a]=a;
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(string a, string b) {
        string root_a = find(a);
        string root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
    
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        //union
        int n=accounts.size();
        for(int i=0;i<n;i++){
            vector<string>& account = accounts[i];
            Union(account[1], to_string(i));//2nd is father
            for(int i=2;i<account.size();i++){
                Union(account[i], account[i-1]);//2nd is father
            }
        }

        unordered_map<string,unordered_set<string>> mp; //nameid_emails
        for(auto& account: accounts){
            for(int i=1;i<account.size();i++){//for each email
                mp[find(account[i])].insert(account[i]);
            }
        }

        vector<vector<string>> res;
        for(auto& [nameid, emails]: mp){
            vector<string> tmp;
            tmp.push_back(accounts[stoi(nameid)][0]);
            for(auto& email: emails){
                tmp.push_back(email);
            }
            sort(tmp.begin()+1, tmp.end());
            res.push_back(tmp);
        }
        return res;
    }
};

/*
accounts: [{name, email0, email1, email2, ...}]

merge rule:
- account with same email -> merge -> guanrantee name same
n- ame can dup for diff people -> use uid_name as identifier
merge res:
- accounts: [{name, email0, email1, email2, ...}] - emails sorted

================================
since grouping relationship is transtive, unionFind to group emails and name into groups
- union operation use name as father to make name finally root
- use uid_name instead of name since name can dup

================================
for each emails
    if in same group -> add to mp<root_id, st>
    for each [root_id, st]
        root_id -> name
        st -> emails, take off name
*/
```