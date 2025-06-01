# 220. Contains Duplicate III

```C++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int indexDiff, int valueDiff) {
        // 1. i!=j
        // 2. index差 <= indexDiff
        // 3. value差 <= valueDiff

        long width = (long)valueDiff + 1;  

        unordered_map<long, long> bucket;  // 以valueDiff為區間，分成一個個buckets，在同一桶的數字表示差值<=valueDiff

        for (int i = 0; i < (int)nums.size(); ++i) {
            long x   = nums[i];    
            long id  = x >= 0 ? x / width : ((x + 1) / width) - 1;

            /* 三種情況可立即返回 true
             (a) 同桶已有元素 → 差值必 ≤ valueDiff
             (b) 左鄰桶存在且 |x - val| ≤ valueDiff
             (c) 右鄰桶存在且 |x - val| ≤ valueDiff                       */
            if (bucket.count(id))                    return true;
            if (bucket.count(id - 1) && llabs(x - bucket[id - 1]) <= valueDiff) return true;
            if (bucket.count(id + 1) && llabs(x - bucket[id + 1]) <= valueDiff) return true;

            bucket[id] = x; 

            /* 維持索引距離 ≤ indexDiff 的滑動視窗
                當 i >= indexDiff，需移除最左側元素                      */
            if (i >= indexDiff) {
                long old   = nums[i - indexDiff];
                long oldId = old >= 0 ? old / width : ((old + 1) / width) - 1;
                bucket.erase(oldId);
            }
        }
        return false;                            // 全走完仍無符合 ⇒ 回傳 false
    }
};
```