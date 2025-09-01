需要有一個加密方式
使用長度家特殊符號

解密時 練習 s.substr(起始位置,長度)
            stoi(字串)

```cpp
class Solution {
public:

    string encode(vector<string>& strs) {
        // 使用長度+# 加密
        string enc;
        for(string str:strs){
            enc+= to_string(str.size())+"#"+str;
        } 
        return enc;
    }

    vector<string> decode(string s) {
        cout<<s;
        vector<string> res;
        int i = 0;
        while (i < s.size()) {
            // 1. 從 i 的位置開始，找到 '#' 的位置
            int j = i;
            while (j < s.size() && s[j] != '#') {
                j++; // 讓 j 前進直到找到 '#'
            }
            
            // 2. 取得長度
            int len = stoi(s.substr(i, j - i));
            
            // 3. 取得字串
            res.push_back(s.substr(j + 1, len));
            
            // 4. 正確更新 i 到下一個區塊的開頭
            i = j + 1 + len;
        }
        return res;
    }
};


```
