# Serialize and Deserialize BST

#: 449
Difficult: Medium
Tags: BFS, BST, DFS, Design, Tree, string
link: https://leetcode.com/problems/serialize-and-deserialize-bst/description/
Priority: Medium
Status: No Idea At First
Created time: June 27, 2023 11:50 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Serialize and Deserialize Binary Tree (Serialize%20and%20Deserialize%20Binary%20Tree%208f74639ba00b413a9bfdce6d698e922b.md)

Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

**Example 1:**

```
Input: root = [2,1,3]
Output: [2,1,3]

```

**Example 2:**

```
Input: root = []
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `0 <= Node.val <= 104`
- The input tree is **guaranteed** to be a binary search tree.

comment:

一开始不知道要考啥，BST和BT有啥区别。

看了答案是要考省略储存NULL node的空间。。

Solution:

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:
    string serialize(TreeNode* root) {
        string s="";
        serializehelper(root, s);
        return s;
    }
    void serializehelper(TreeNode* root, string& s){
        if(!root) return;
        s+=to_string(root->val) + ","; 
        serializehelper(root->left, s);
        serializehelper(root->right, s);
    }
    
    TreeNode* deserialize(string data) {
        return deserializehelper(data, INT_MIN, INT_MAX);
    }
    TreeNode* deserializehelper(string& data, int min, int max) {
        if(data.size()==0) return NULL;
        
        int pos=data.find(",");
        int val=stoi(data.substr(0,pos));
        if (val < min || val > max) return NULL;
        data=data.substr(pos+1);
        
        TreeNode* node = new TreeNode(val); 
        node->left=deserializehelper(data, min, node->val);
        node->right=deserializehelper(data, node->val, max);
        return node;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec* ser = new Codec();
// Codec* deser = new Codec();
// string tree = ser->serialize(root);
// TreeNode* ans = deser->deserialize(tree);
// return ans;
```