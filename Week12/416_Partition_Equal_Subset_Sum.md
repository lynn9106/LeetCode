# 416. Partition Equal Subset Sum

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int total = 0;
        for (int num : nums) total += num;

        // if the sum is odd, we can't divide into 2
        if (total % 2 != 0) return false;

        int target = total / 2;
        vector<bool> dp(target + 1, false); // dp[i] = true: a subset's sum is i
        dp[0] = true;  // base case: if we don't choose any number we get 0

        for (int num : nums) {
            // from back to front to ensure every num used once
            for (int j = target; j >= num; --j) {
                if (dp[j - num])
                    dp[j] = true;
            }
        }

        return dp[target];
    }
};
```