# 2742. Painting the Walls

```C++
class Solution {
public:
    int paintWalls(vector<int>& cost, vector<int>& time) {
        int n = cost.size();    // n walls
        
        const int INF = 1e9;
        
        vector<int> dp(n + 1, INF);     // to calculate dp[k]: how much we have to cost for k walls
        dp[0] = 0;

        for (int i = 0; i < n; ++i) {       // for each wall we consider if we should pay for it
            for (int j = n; j >= 0; --j) {
                int new_j = min(n, j + time[i] + 1);    // if we finished j walls, and now pay for painting wall i, we can finish j+time[i]+1 wall, and we at most just need to paint n walls
                dp[new_j] = min(dp[new_j], dp[j] + cost[i]);    // try to update the cost for new_j, of paying for wall i after spendng dp[j] will cost less
            }
        }

        return dp[n];
    }
};
```