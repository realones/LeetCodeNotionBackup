# Design In-Memory File System

#: 588
Difficult: Hard
Tags: Design, Hash, Trie, string
link: https://leetcode.com/problems/design-in-memory-file-system
Priority: Medium
Status: HardToImplement
Created time: November 17, 2023 1:29 AM
Last edited time: November 17, 2023 12:46 PM
practicedTimes: 1
source: ticktokFreq

Design a data structure that simulates an in-memory file system.

Implement the FileSystem class:

- `FileSystem()` Initializes the object of the system.
- `List<String> ls(String path)`The answer should in **lexicographic order**.
    - If `path` is a file path, returns a list that only contains this file's name.
    - If `path` is a directory path, returns the list of file and directory names **in this directory**.
- `void mkdir(String path)` Makes a new directory according to the given `path`. The given directory path does not exist. If the middle directories in the path do not exist, you should create them as well.
- `void addContentToFile(String filePath, String content)`
    - If `filePath` does not exist, creates that file containing given `content`.
    - If `filePath` already exists, appends the given `content` to original content.
- `String readContentFromFile(String filePath)` Returns the content in the file at `filePath`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/28/filesystem.png](https://assets.leetcode.com/uploads/2021/04/28/filesystem.png)

```
Input
["FileSystem", "ls", "mkdir", "addContentToFile", "ls", "readContentFromFile"]
[[], ["/"], ["/a/b/c"], ["/a/b/c/d", "hello"], ["/"], ["/a/b/c/d"]]
Output
[null, [], null, null, ["a"], "hello"]

Explanation
FileSystem fileSystem = new FileSystem();
fileSystem.ls("/");                         // return []
fileSystem.mkdir("/a/b/c");
fileSystem.addContentToFile("/a/b/c/d", "hello");
fileSystem.ls("/");                         // return ["a"]
fileSystem.readContentFromFile("/a/b/c/d"); // return "hello"

```

**Constraints:**

- `1 <= path.length, filePath.length <= 100`
- `path` and `filePath` are absolute paths which begin with `'/'` and do not end with `'/'` except that the path is just `"/"`.
- You can assume that all directory names and file names only contain lowercase letters, and the same names will not exist in the same directory.
- You can assume that all operations will be passed valid parameters, and users will not attempt to retrieve file content or list a directory or file that does not exist.
- `1 <= content.length <= 50`
- At most `300` calls will be made to `ls`, `mkdir`, `addContentToFile`, and `readContentFromFile`.

```cpp
class Node {
public:
    map<string, Node*> nexts;
    Node():nexts({}){}
};

class FileSystem {
public:
    Node* root;
    unordered_map<string,string> files;

    FileSystem() {
        root= new Node();// root path /
    }
    
    vector<string> ls(string path) {
        if(files.count(path)){
            int idx=path.find_last_of('/');
            return {path.substr(idx+1)};
        }

        vector<string> p=s2v(path);
        Node *node=root;
        for(auto& name: p){
            node=node->nexts[name];
        }
        vector<string> res;
        for(auto& [name,_]: node->nexts){
            res.push_back(name);
        }
        return res;
    }
    
    void mkdir(string path) {
        vector<string> p=s2v(path);
        Node *node=root;
        for(auto& name: p){
            if(node->nexts[name]==NULL){
                node->nexts[name]=new Node();
            }
            node=node->nexts[name];
        }
        return;
    }
    
    void addContentToFile(string filePath, string content) {
        mkdir(filePath);
        files[filePath]+=content;
    }
    
    string readContentFromFile(string filePath) {
        return files[filePath];
    }

    vector<string> s2v(string& s){//s start with /
        vector<string> res;
        for(int l=1,rr=1;l!=string::npos && rr!=s.size();){
            l=s.find_first_not_of("/",rr); if(l==string::npos) break;
            rr=s.find_first_of("/",l); if(rr==string::npos) rr=s.size();
            res.push_back(s.substr(l, rr-l));
        }
        return res;
    }
};

/*
len: 1~100
content: 1~50
at most 300 calls
*/

/**
 * Your FileSystem object will be instantiated and called as such:
 * FileSystem* obj = new FileSystem();
 * vector<string> param_1 = obj->ls(path);
 * obj->mkdir(path);
 * obj->addContentToFile(filePath,content);
 * string param_4 = obj->readContentFromFile(filePath);
 */
```