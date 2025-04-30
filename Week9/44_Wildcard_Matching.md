# 44. Wildcard Matching

```C++
#include <vector>
#include <string>
using namespace std;

class Solution {
public:
    bool isMatch(string s, string p) {      // can p fully match s?
        int n = s.size(), m = p.size();
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false)); // dp[i][j] denotes if p[0..i-1] can fully match s[0..j-1]
        
        dp[0][0] = true; // empty strings

        for (int i = 1; i <= m; ++i) {  // s is an empty string and only * can match => p is all *
            if (p[i - 1] == '*')
                dp[i][0] = true;
            else
                break;  // if the char is not *, can't match empty string, break (default: false)
        }

        for (int i = 1; i <= m; ++i) {      // for the reamin dp[i][j]
            for (int j = 1; j <= n; ++j) {
                if (p[i - 1] == s[j - 1] || p[i - 1] == '?') {  //if the current char can match the s[j-1]
                    dp[i][j] = dp[i - 1][j - 1];    // if the char before is matched, then current will be matched
                } else if (p[i - 1] == '*') {   // or if current char is a *
                    dp[i][j] = dp[i - 1][j] || dp[i][j - 1];    // we can take it as an empty string = dp[i-1][j] or we can take it as any char and consider if the early one match 
                }
            }
        }

        return dp[m][n];
    }
};



/*
            s         0     1     2
            j → 0     1     2     3  （代表 s 的長度）
     p  i ↓     ""    a     a     b
            +-----+-----+-----+-----+
        0 "" |  T  |  F  |  F  |  F  |
     0  1 *  |  T  |  T  |  T  |  T  |
     1  2 b  |  F  |  F  |  F  |  T  |
*/


```