# 322. Coin Changed

```C++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        // dp for recording the least coins for amount i
        // initial dp as amount+1 (an impossible value)
        vector<int>  dp(amount+1, amount+1);
        dp[0] = 0;

        for (int i =1; i <= amount; i++){
            for (int j = 0; j< coins.size(); j++)  // try every coin
                if(i - coins[j] >= 0)
                dp[i] = min(dp[i], dp[i - coins[j]] + 1);   //find the mininum nuber for i-coin, and add 1 (use one coin[j])
        }

        return dp[amount] == amount + 1 ? -1 : dp[amount];       //If that amount of money cannot be made up by any combination of the coins, it will remain default


    }
};
```