使用並查集以及set 來儲存最小活躍的id

```cpp
#include <iostream>
#include <vector>
#include <numeric>
#include <set>
#include <algorithm>

using namespace std;

// --- LeetCode 格式開始 ---

class Solution {
public:
    /**
     * @brief 解決 Pod 連線與失效問題的核心函式
     * @param n Pod 的總數，編號從 1 到 n
     * @param connections Pod 之間的直接連線
     * @param queries 一系列的查詢操作
     * @return vector<int> 回傳所有類型 1 查詢的結果
     */
    
    // 並查集 存活躍的ID
    // 使用set 來追蹤每個區域的最小活躍ID
    
    vector<int>DSU;
    vector<set<int>> active_pods_in_region;
    
    void init(int n){
        DSU.resize(n+1);
        active_pods_in_region.resize(n + 1);
        for(int i=1; i<=n; i++){
            DSU[i]=i;
            active_pods_in_region[i].insert(i); // 每個 Pod 一開始自成一區，且是活躍的
        }
    }
    int find(int u){
        if(DSU[u]==u)return u;
        DSU[u] = find(DSU[u]);
        return DSU[u] ;
    }
    
    void join(int u ,int v){
        int rootu = find(u);
        int rootv = find(v);
        if(rootu==rootv)return;
        
        
        if (active_pods_in_region[rootu].size() < active_pods_in_region[rootv].size()) {
            swap(rootu, rootv);
        }
    
        if(rootu!=rootv){
            DSU[rootv] = rootu;
            // u當根
            active_pods_in_region[rootu].insert(
                active_pods_in_region[rootv].begin(),
                active_pods_in_region[rootv].end()    
            );
            
            active_pods_in_region[rootv].clear();
            
        }
        
    }
    
    
    vector<int> recoverDeadPods(int n, vector<vector<int>>& connections, vector<vector<int>>& queries) {
        // ===============================================================
        // ============ 在這裡開始撰寫您的程式碼 ============
        // ===============================================================

        // 提示：
        // 1. 您可能需要一個並查集 (DSU) 的結構來管理區域。
        // 2. 您需要一個方法來追蹤每個區域的「最小活躍 ID」。
        //    (可以考慮為 DSU 的每個根節點搭配一個 std::set)
        init(n);
        vector<int> results;
        
        for(auto &pair : connections){
            int u = pair[0];
            int v = pair[1];
            join(u,v);
            
        }
        
        for(int i=0;i<queries.size();i++){
            int type = queries[i][0];
            int pod_id = queries[i][1];
            int root = find(pod_id);
            
            if(type==2){
                active_pods_in_region[root].erase(pod_id);
            }
            // 查詢 但有可能失效
            if(type==1){
                if(active_pods_in_region[root].empty()){
                    results.push_back(-1);
                }
                else{
                    results.push_back(*active_pods_in_region[root].begin());
                }
            }
        }
        
        return results;

        // ===============================================================
        // ============ 程式碼結束 ============
        // ===============================================================
    }
};

// --- LeetCode 格式結束 ---

// 輔助函式：用於印出 vector
void print_vector(const string& title, const vector<int>& vec) {
    cout << title << "[";
    for (size_t i = 0; i < vec.size(); ++i) {
        cout << vec[i] << (i == vec.size() - 1 ? "" : ", ");
    }
    cout << "]" << endl;
}

// 主函式，用於測試您的 Solution
int main() {
    Solution sol;

    // --- 測試案例 1：基本範例 ---
    cout << "--- 測試案例 1：基本範例 ---" << endl;
    int n1 = 3;
    vector<vector<int>> connections1 = {{1, 2}, {2, 3}};
    vector<vector<int>> queries1 = {{2, 2}, {1, 2}, {2, 3}, {2, 1}, {1, 1}};
    vector<int> expected1 = {1, -1};
    vector<int> result1 = sol.recoverDeadPods(n1, connections1, queries1);
    print_vector("預期輸出: ", expected1);
    print_vector("您的輸出: ", result1);
    cout << endl;

    // --- 測試案例 2：多個獨立區域 ---
    cout << "--- 測試案例 2：多個獨立區域 ---" << endl;
    int n2 = 6;
    vector<vector<int>> connections2 = {{1, 2}, {4, 5}, {5, 6}};
    vector<vector<int>> queries2 = {{1, 1}, {1, 3}, {1, 6}, {2, 1}, {1, 2}, {2, 5}, {1, 4}};
    vector<int> expected2 = {1, 3, 4, 2, 4};
    vector<int> result2 = sol.recoverDeadPods(n2, connections2, queries2);
    print_vector("預期輸出: ", expected2);
    print_vector("您的輸出: ", result2);
    cout << endl;

    // --- 測試案例 3：最低 ID 節點失效 ---
    cout << "--- 測試案例 3：最低 ID 節點失效 ---" << endl;
    int n3 = 4;
    vector<vector<int>> connections3 = {{1, 2}, {1, 3}, {1, 4}};
    vector<vector<int>> queries3 = {{1, 4}, {2, 1}, {1, 3}, {2, 2}, {1, 4}};
    vector<int> expected3 = {1, 2, 3};
    vector<int> result3 = sol.recoverDeadPods(n3, connections3, queries3);
    print_vector("預期輸出: ", expected3);
    print_vector("您的輸出: ", result3);
    cout << endl;

    // --- 測試案例 4：整個區域完全失效 ---
    cout << "--- 測試案例 4：整個區域完全失效 ---" << endl;
    int n4 = 2;
    vector<vector<int>> connections4 = {};
    vector<vector<int>> queries4 = {{1, 1}, {2, 1}, {1, 1}, {2, 2}, {1, 2}};
    vector<int> expected4 = {1, -1, -1};
    vector<int> result4 = sol.recoverDeadPods(n4, connections4, queries4);
    print_vector("預期輸出: ", expected4);
    print_vector("您的輸出: ", result4);
    cout << endl;

    return 0;
}

```
