# 1209. Remove All Adjacent Duplicates in String II

```C++
class Solution {
public:
    string removeDuplicates(string s, int k) {
        // pair<char,int>：first 為字元，second 為目前連續出現次數
        vector<pair<char,int>> stack;          // 建立stack

        for (char c : s) {                  // 依序確認各個字元
            if (stack.empty() || stack.back().first != c) {
                stack.push_back({c, 1}); // stack空 或 與stack最後字元不同 ⇒ 新字元，count 1
            } else {      
                ++stack.back().second;     // 與最後字元相同 ⇒ 更新count
                if (stack.back().second == k) {
                    stack.pop_back();      // 達到 k 次 ⇒ 整段刪除（從堆疊移除）
                }
            }
        }

        // 將stack內容還原成最終答案
        string ans;
        ans.reserve(s.size());
        for (auto &p : stack) {
            ans.append(p.second, p.first);
        }
        return ans;
    }
};
```