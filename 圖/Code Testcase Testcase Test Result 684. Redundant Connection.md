找到一條邊
他的兩個點都有被加入集合中
所以使用並查集

```cpp

class Solution {
public:
    vector<int>Father;
    void init(int n){
        Father.resize(n);
        for(int i=0;i<n;i++){
            Father[i]=i;
        }
    }
    
    int find(int u){
        if(u==Father[u])return u;
        Father[u] = find(Father[u]);
        return Father[u];
    }

    void join(int u,int v){
        int root_u = find(u);
        int root_v = find(v);
        if(root_u==root_v)return;
        Father[root_u]=root_v;
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        init(n+1);

        for(auto vec:edges){
            int a = vec[0];
            int b = vec[1];
            if(find(a)==find(b))return {a,b};
            join(a,b);
        }    
        return {0,0};
    }
};

```
