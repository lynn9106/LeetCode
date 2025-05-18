# 847. Shortest Path Visiting All Nodes

```C++
class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        int n = graph.size();
        if (n == 1) return 0;

        // visited[i][mask] = true : node i is visited at mask state
        // mask: n bitmask (eg. 01101: visit 0、2、3）
        vector<vector<bool>> visited(n, vector<bool>(1 << n, false));
        queue<tuple<int, int, int>> q; // (cur_node, mask, steps)

        // Step 1: initial BFS start form every node
        for (int i = 0; i < n; ++i) {
            int mask = (1 << i); // only visit node i
            q.push({i, mask, 0});
            visited[i][mask] = true;
        }

        // every node has been visited
        int full_mask = (1 << n) - 1;

        // Step 2: start BFS
        while (!q.empty()) {
            auto [node, mask, steps] = q.front();
            q.pop();

            for (int nei : graph[node]) {
                int next_mask = mask | (1 << nei); // add visit nei
                if (next_mask == full_mask) {
                    return steps + 1; // all node has been visited return length
                }

                if (!visited[nei][next_mask]) {
                    visited[nei][next_mask] = true;
                    q.push({nei, next_mask, steps + 1});
                }
            }
        }

        return -1;
    }
};

```