# 316. remove-duplicate-letters

```C++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    string removeDuplicateLetters(string s) {
        int n = s.size();
        
        /* step1. 計算每一個字元出現幾次 */
        vector<int> remain(26, 0);  
        for (char c : s) ++remain[c - 'a'];
        
        vector<bool> inStack(26, false);    // 記錄字元是否已在stack中
        string stack;              
        
        /* step2. 遍歷整個字串，維護「嚴格遞增」，確保字典序最小 */
        for (char c : s) {
            int idx = c - 'a';
            --remain[idx];                  // 這個 c 已經被處理，剩餘次數 -1
            
            if (inStack[idx])               // 如果已經在 stack 中，直接跳過
                continue;
            
            /* 若當前字元 c 比現在stack最後的字元小，且該數之後還會出現 (remain>0)，
               可以把先pop改成先放c，以獲得更小字典序 */
            while (!stack.empty() && c < stack.back() && remain[stack.back() - 'a'] > 0) {
                inStack[stack.back() - 'a'] = false;
                stack.pop_back(); 
            }
            
            // push 目前字元進 stack，並標記已在答案中
            stack.push_back(c);
            inStack[idx] = true;
        }
        
        return stack;  // stack 內容為最終答案
    }
};
```