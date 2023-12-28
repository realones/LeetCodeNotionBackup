# Serialize and Deserialize Binary Tree

#: 297
Difficult: Hard
Tags: BFS, Design, Divide and Conquer, Iterate, Tree, tbfs_postChkNull_soSz
link: https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
Priority: High
Created time: July 17, 2022 5:22 PM
Last edited time: October 16, 2023 3:56 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初BFS
related: Serialize and Deserialize BST (Serialize%20and%20Deserialize%20BST%20ba35b9d29fc64212a2c4d2973c8f0da8.md)

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![Untitled](Serialize%20and%20Deserialize%20Binary%20Tree%208f74639ba00b413a9bfdce6d698e922b/Untitled.png)

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]

```

**Example 2:**

```
Input: root = []
Output: []

```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `1000 <= Node.val <= 1000`

preOrder and inOrder

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front(); q.pop();
						if(!res.empty()) res+=","
            if(!cur){
                res+="#";
                continue;
            }
            res+=to_string(cur->val);
            q.push(cur->left);
            q.push(cur->right);
        }
        res=res.substr(1);
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        TreeNode* root = pophead(data);
        if(!root) return NULL;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* cur=q.front(); q.pop();
            if(!cur){
                continue;
            }
            cur->left=pophead(data);
            cur->right=pophead(data);
            q.push(cur->left);
            q.push(cur->right);
        }
        return root;
    }

    TreeNode* pophead(string& s){
        int pos=s.find(",");
        string res=s.substr(0,pos);
        s=s.substr(pos+1);
        // std::cout << res << std::endl;
        if(res=="#"){return NULL;}
        return new TreeNode(stoi(res));
    }
};
```

```cpp
class Solution {
public:
    
    string serialize(TreeNode * root) {
        if(!root) return "#";
        //pre order
        return to_string(root->val)+","+serialize(root->left)+","+serialize(root->right);
    }

    
    TreeNode * deserialize(string &data) {
        string cur=pophead(data);
        if(cur=="#") return NULL;
        
        TreeNode* node=new TreeNode(stoi(cur));
        node->left=deserialize(data);
        node->right=deserialize(data);
        
        return node;
    }
    
    string pophead(string& s){
        int pos=s.find(",");
        string res=s.substr(0,pos);
        s=s.substr(pos+1);
        return res;
    }
};
```