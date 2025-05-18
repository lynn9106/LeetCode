# 685. Redundant Connection II

```C++
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1, 0);
        vector<int> e1, e2;

        // Step 1: Check if there's a node with two parents
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            if (parent[v] == 0) {
                parent[v] = u;
            } else {
                // Found a node with two parents
                e1 = {parent[v], v}; // earlier one
                e2 = {u, v};         // later one
                edge[1] = 0;  // Mark this edge to ignore later
            }
        }

        // Step 2: Union-Find to check for cycle
        for (int i = 0; i <= n; ++i)        // default every node as its parent
            parent[i] = i;

        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            if (v == 0) continue;  // skip the second parent edge (e2)

            int pu = find(parent, u);
            int pv = find(parent, v);
            if (pu == pv) {
                // u and v is in same set, and they are going to make an edge -> Found a cycle
                return e1.empty() ? edge : e1;    // if e1 cause the cycle, remove e1
            }
            parent[pv] = pu;
        }

        // Step 3: No cycle found, so it's a two-parent case, delete e2
        return e2;
    }

private:
    int find(vector<int>& parent, int x) {
        if (parent[x] != x)
            parent[x] = find(parent, parent[x]);
        return parent[x];
    }
};



```