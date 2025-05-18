# 887. Super Egg Drop

```C++
class Solution {
public:
    int superEggDrop(int K, int N) {
        vector<int> dp(K + 1, 0);   // at m moves, k eggs can test how many floors
        int m;
        for (m = 0; dp[K] < N; m++) // test from m=1 until the dp[K] can test N floors
            for (int k = K; k > 0; --k)
                dp[k] += dp[k - 1] + 1; //new_dp[k] = prev_dp[k](egg didn't break) + prev_dp[k - 1](egg break) + 1
        return m;
    }
};
```