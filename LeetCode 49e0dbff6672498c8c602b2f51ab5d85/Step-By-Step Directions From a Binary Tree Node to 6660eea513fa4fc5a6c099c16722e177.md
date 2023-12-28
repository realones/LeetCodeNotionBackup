# Step-By-Step Directions From a Binary Tree Node to Anothe

#: 2096
Difficult: Medium
Tags: Divide and Conquer, Tree
link: https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/
Priority: Medium
Created time: October 1, 2022 1:25 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

You are given the `root` of a **binary tree** with `n` nodes. Each node is uniquely assigned a value from `1` to `n`. You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the **shortest path** starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the **uppercase** letters `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

- `'L'` means to go from a node to its **left child** node.
- `'R'` means to go from a node to its **right child** node.
- `'U'` means to go from a node to its **parent** node.

Return *the step-by-step directions of the **shortest path** from node* `s` *to node* `t`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/11/15/eg1.png](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)

```
Input: root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
Output: "UURL"
Explanation: The shortest path is: 3 → 1 → 5 → 2 → 6.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/11/15/eg2.png](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)

```
Input: root = [2,1], startValue = 2, destValue = 1
Output: "L"
Explanation: The shortest path is: 2 → 1.

```

**Constraints:**

- The number of nodes in the tree is `n`.
- `2 <= n <= 105`
- `1 <= Node.val <= n`
- All the values in the tree are **unique**.
- `1 <= startValue, destValue <= n`
- `startValue != destValue`

**Solution**:

very easy to MLE, unless reverse the path generation

Solution1:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    string getDirections(TreeNode* root, int startValue, int destValue) {
        /*
        find directions for both start and end from root
        remove common prefix path
        replace all steps in start directin to "U" and aadd destination direction
        */
        string startDirection, destDirection;
        gotoDest(root, startValue, startDirection);
        gotoDest(root, destValue, destDirection);
        while(!startDirection.empty() && !destDirection.empty() && 
             startDirection.back()==destDirection.back()){
            startDirection.pop_back();
            destDirection.pop_back();
        }
        reverse(destDirection.begin(),destDirection.end());
        return string(startDirection.size(),'U')+destDirection;
    }
    
    //reverse direction order, otherwise will MLE
    bool gotoDest(TreeNode* root, int value, string& res){
        if(!root) return false;//invalid
        if(root->val==value) return true;
        bool l=gotoDest(root->left,value, res);
        if(l) {res+='L'; return true;}
        bool r=gotoDest(root->right,value, res);
        if(r) {res+='R'; return true;}
        return false;
    }
};
```

Solution2

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    string getDirections(TreeNode* root, int startValue, int destValue) {
        /*
        find common ancestor
        from start to ancestor-> U
        from ancestor to end -> L or R
        */
        TreeNode* commonRoot=findCommonAncestor(root, startValue, destValue);
        string res;
        int distance=findDistance(commonRoot, startValue);
        res+=string(distance,'U');
        string path2;
        gotoDest(commonRoot, destValue,path2);
        reverse(path2.begin(),path2.end());
        res+=path2;
        return res;
    }
    
    TreeNode* findCommonAncestor(TreeNode* root, int startValue, int destValue){
        if(!root) return NULL;
        if(root->val==startValue || root->val==destValue) return root;
        TreeNode* l=findCommonAncestor(root->left,startValue,destValue);
        TreeNode* r=findCommonAncestor(root->right,startValue,destValue);
        if(l&&r) return root;
        if(l) return l;
        if(r) return r;
        return NULL;
    }
    
    int findDistance(TreeNode* root, int value){
        if(!root) return -1;
        if(root->val==value){return 0;}
        int l=findDistance(root->left,value);
        if(l!=-1) return l+1;
        int r=findDistance(root->right,value);
        if(r!=-1) return r+1;
        return -1;
    }
    
    //return the reversed path order, otherwise MLE
    bool gotoDest(TreeNode* root, int value, string& res){
        if(!root) return false;//invalid
        if(root->val==value) return true;
        bool l=gotoDest(root->left,value, res);
        if(l) {res+="L"; return true;}
        bool r=gotoDest(root->right,value, res);
        if(r) {res+="R"; return true;}
        return false;
    }
    
    // string gotoDest(TreeNode* root, int value){
    //     if(!root) return "#";//invalid
    //     if(root->val==value) return "";
    //     string l=gotoDest(root->left,value);
    //     string r=gotoDest(root->right,value);
    //     if(l=="#" && r=="#") return "#";
    //     return l=="#"? "R"+r: "L"+l;
    // }
};
```