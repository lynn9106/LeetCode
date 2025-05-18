# 312. Burst Balloons

```C++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n = nums.size();

        // add balloons of 1 at front and back
        vector<int> balloons(n + 2);
        balloons[0] = balloons[n + 1] = 1;
        for (int i = 0; i < n; ++i) {
            balloons[i + 1] = nums[i];
        }

        // dp[i][j] represent the most coins of bursting the balloons between (i, j)
        vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));

        for (int length = 2; length <= n + 1; ++length) {   // start from the shortest interval
            for (int left = 0; left + length <= n + 1; ++left) {    // from interval left to right
                int right = left + length;
                for (int k = left + 1; k < right; ++k) {    // all the balloons k in the interval
                    dp[left][right] = max(
                        dp[left][right],
                        dp[left][k] + dp[k][right] + balloons[left]*balloons[k]*balloons[right]
                        // if the last one busrt is k, then coins = maxOfLeftInterval + maxOfRightInterval + the coins of bursting k
                    
                    );
                }
            }
        }

        return dp[0][n + 1]; // the most coins of bursting the whole interval (0, n+1) 
    }
};

```