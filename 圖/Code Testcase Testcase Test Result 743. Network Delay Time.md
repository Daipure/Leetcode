dijkstra 演算法 


```cpp

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        // dijkstra 演算法 
        // 1. 選擇離原點距離最近的點
        // 2. 標記為訪問過
        // 3. 更新未訪過節點的 min_dis

        vector<bool>visited(n+1,false);
        vector<int> min_dis(n+1,INT_MAX);
        vector<vector<int>>grid(n+1,vector<int>(n+1,-1));
        
        for(int i=0;i<times.size();i++){
            int u = times[i][0]; 
            int v = times[i][1];
            int w = times[i][2];
            grid[u][v]=w;
        }

        // 初始化起點
        min_dis[k] = 0;

        for(int i=1;i<=n;i++){
            int min_val = INT_MAX;
            int cur_index = -1;
            for(int j=1;j<=n;j++){
                if(!visited[j] && min_dis[j] < min_val){
                    min_val = min_dis[j];
                    cur_index = j;
                }
            }
            if (cur_index == -1) {
                break;
            }
            visited[cur_index] = true;

            for(int j=1;j<=n;j++){
                // 更新未訪過節點的 min_dis
                if(grid[cur_index][j] != -1 && !visited[j] && min_dis[cur_index]!=INT_MAX && min_dis[cur_index]+grid[cur_index][j]<min_dis[j]){
                    min_dis[j] = min_dis[cur_index]+grid[cur_index][j]; 
                }
            }
        }

        int max_val = INT_MIN;

        for(int i=1;i<=n;i++){
            if(min_dis[i]>=max_val){
                max_val = min_dis[i];
            }

        }
        return max_val == INT_MAX?-1:max_val;
    }
};

```
