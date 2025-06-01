# 834. Sum of Distances in Tree

```C++
class Solution {
public:
    int n;
    vector<vector<int>> tree;   // tree[u] = all neighbor node directly connect with u
    vector<int> res;    // res[u] = sum of distances from u to all nodes
    vector<int> count;  // count[u] = the node numbers of subtree from root u (include the root)

    // dfs1：assume the node 0 as root, do dfs
    void dfs1(int u, int parent, int depth) {       // current node:u, parent node, depth to 0 
        res[0] += depth;        // add the distance from current node u to node 0 to res[0]
        for (int v : tree[u]) {     // scan every neighbor of u
            if (v == parent) continue;  // skip the edge to parent node
            dfs1(v, u, depth + 1);  // get into descendents v, and the depth +1
            count[u] += count[v];   // after the descendent retrun, add discendents' subtree size to count[u]
        }
    }

    // dfs2：rerooting，get res[v] from parent node u
    void dfs2(int u, int parent) {
        for (int v : tree[u]) {  // scan every neighbor of u
            if (v == parent) continue;  // skip the edge to parent node

            // re-rootung:
            // if we change the root from u to v
            // for subtree nodes of v, the distance is decrease 1
            // for the remain (n- count[v]) nodes, the distance will increase 1
            res[v] = res[u] - count[v] + (n - count[v]);    
            dfs2(v, u);     // do rerooting for v
        }
    }

    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {
        this->n = n;
        tree.assign(n, {});     // establish n empty list
        for (auto &e : edges) {     // get the neighbors from the edges
            int u = e[0], v = e[1];     // the two nodes are connected
            tree[u].push_back(v);
            tree[v].push_back(u);
        }

        res.assign(n, 0);   // initial res as all 0
        count.assign(n, 1); // initial count as all 1 (every node have themselves)
        dfs1(0, -1, 0);     // first round dfs: take 0 as root, calculate res[0] & count[u]
        dfs2(0, -1);        // second round dfs: rerooting to get res[u]

        return res;
    }
};
```