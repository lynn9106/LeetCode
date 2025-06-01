# 347. Top K Frequent Elements

```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> freq;          // 紀錄各數字出現次數
        int maxfreq = 0;
        for (int x : nums)    
        {
            ++freq[x];
            if (freq[x]> maxfreq)
                maxfreq = freq[x];

        }                                   

        vector<vector<int>> bucket(maxfreq + 1);    // bucket[次數] = 所有為該頻率數字
        for (auto& [num, f] : freq)
            bucket[f].push_back(num);         // 放入對應頻率的bucket

        vector<int> ans;                    
        for (int f = maxfreq; f > 0 && ans.size() < k; --f) {
            for (int num : bucket[f]) {       // 從頻率高到低加入數字
                ans.push_back(num);       
                if ((int)ans.size() == k)     // 若已收滿 k 個就結束
                    break;
            }
        }
        return ans;               
    }
};
```