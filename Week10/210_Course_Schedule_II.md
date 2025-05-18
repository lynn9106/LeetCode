# 210. Course Schedule II

```C++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        // cycle -> false
        
        vector<vector<int>> graph(numCourses);  // the relation between cources 
        vector<int> inDegree(numCourses, 0);    // indegree of course i, represent of how many prerequisities

        for (const auto& prereq : prerequisites) {      // process each related course pair
            int course = prereq[0];
            int preCourse = prereq[1];
            graph[preCourse].push_back(course);  // preCourse -> course
            inDegree[course]++;
        }

        // initialize queue
        queue<int> q;
        for (int i = 0; i < numCourses; ++i) {
            if (inDegree[i] == 0)       // put courses that are 0 indegree, no prereq
                q.push(i);
        }

        vector<int> order;

        while (!q.empty()) {       
            int curr = q.front();    // get a course that can take
            q.pop();
            order.push_back(curr);

            for (int neighbor : graph[curr]) {      // after taking the course, the prereq of the next course -1
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0)    // if the course's prereq are all taken, add in queue
                    q.push(neighbor);
            }
        }

        // if the number of courses in the order isnot numCourses -> there's cycle, some course will never be prereq = 0
        if (order.size() != numCourses)
            return {};

        return order;
    }
};
```