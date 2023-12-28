# union find

Tags: Matrix, UnionFind

Matrix version

```cpp
//union find
using ii=pair<int,int>;
//init
vector<vector<ii>> father;//since ii doesn't have hash func, use vv instead
void init(int m, int n) {//not required if using hash_map, init in begining of find()
    father.resize(m,vector<ii>(n));
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            father[i][j]={i,j};
        }
    }
}

//with pass compression
// O(1)
ii find(ii cell){
    auto [x,y]=cell;
    if (father[x][y] == cell) return cell;
    father[x][y] = find(father[x][y]); //pass compression
    return father[x][y];
}

//union - union roots, tree like
// O(1)
bool Union(ii a, ii b) {
    ii root_a = find(a);
    ii root_b = find(b);
    if (root_a != root_b){
        father[root_a.first][root_a.second] = root_b;
        return true;
    }
    return false;
}
```