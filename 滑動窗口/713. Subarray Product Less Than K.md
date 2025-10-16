找出連續子陣列 符合乘積嚴格小於k的所有可能
當 [l,r] 符合 [l+1,r] 也會符合
right - left + 1 計算的是：在當前 right 指針的位置上，有多少個以 nums[right] 結尾的子陣列，其乘積是小於 k 的。

```cpp

class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        // 找出連續子陣列 符合乘積嚴格小於k的所有可能
        // 當 [l,r] 符合 [l+1,r] 也會符合

        int res = 0;
        int product = 1;
        int left = 0;

        if(k<=1)return 0;

        for(int right=0;right<nums.size();right++){
            product = product * nums[right];    
            while(product>=k){
                product/=nums[left];
                left++;
            }
            // right - left + 1 計算的是：在當前 right 指針的位置上，有多少個以 nums[right] 結尾的子陣列，其乘積是小於 k 的。
            res += right-left+1;
        }
        return res;
    }
};
```
