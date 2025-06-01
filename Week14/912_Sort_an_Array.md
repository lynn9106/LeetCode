# 2071. Maximum Number of Tasks You Can Assign

```C++
class Solution {
public:
    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(),   tasks.end());      // 任務由易到難
        sort(workers.begin(), workers.end());    // 工人由弱到強

        /* ---------- 1. binary search on k ---------- */
        int lo = 0, hi = min(tasks.size(), workers.size()), ans = 0;    //最多只能完成task數量或工人數量 的task

        while (lo <= hi) {
            int mid = (lo + hi) / 2;            // 嘗試做 mid 件工作
            if (canAssign(mid, tasks, workers, pills, strength))
                ans = mid, lo = mid + 1;        // 至少可以做mid件，嘗試更多
            else
                hi = mid - 1;                   // 不行做mid件，減少數量
        }
        return ans;
    }

private:
    /* 判斷能否完成 k 個任務 */
    bool canAssign(int k, const vector<int>& tasks, const vector<int>& workers, int pills, int strength) {
        if (k == 0) return true;

        int w = workers.size() - 1;              // 指向目前最強工人
        deque<int> boosted;                      // 吃藥後可以指派的工人

        /* 從最難的 task 開始分配 */
        for (int t = k - 1; t >= 0; --t) {
            int need = tasks[t];                 // 此任務需求
            
            /* 1. 如果有不需藥就可指派的工人 */
            if (w >= 0 && workers[w] >= need) {
                --w;  
                continue;
            }


            /* 2. 如果boosted的front(在上一個任務吃藥後最強的工人)可以不吃藥完成該任務，則指派*/
            if (!boosted.empty() && boosted.front() >= need) {
                boosted.pop_front();    
                continue;
            }

            /* 3. 否則把目前還沒用過、吃藥後能完成任務的工人加入boosted */
            while (w >= 0 && workers[w] + strength >= need) {
                boosted.push_back(workers[w]);
                --w;
            }

            // 若沒有可用的工人，或藥丸其實已經用完 ⇒ 無法完成該任務
            if (boosted.empty() || pills == 0) return false;

            // 給藥後可以完成該任務，給最弱但吃藥後還夠的工人一顆藥，完成任務
            boosted.pop_back();
            --pills;
        }
        return true;                             // k 個任務全部完成
    }
};
```