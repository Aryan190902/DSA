**Question**: [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

**Approach**:
- Model courses as nodes and each prerequisite pair as a directed edge from the prerequisite course to the dependent course.
- Build the adjacency list and count the indegree of every course.
- Start with all courses having indegree zero and process them using Kahn's algorithm.
- Remove each course from the queue, append it to the answer, and decrement the indegrees of its dependent courses.
- Return the resulting order if it contains every course; otherwise, a cycle exists and no valid course order is possible.

TC: `O(V + E)`, where $V = numCourses$ and $E$ is the number of prerequisite pairs.
SC: `O(V + E)` for the graph and indegree array. The answer and queue add up to `O(V)` auxiliary/output space.

**Code**:
```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> indeg(numCourses, 0);
        vector<vector<int>> adj(numCourses);

        // Each pair [a, b] means b must be completed before a.
        for (const vector<int>& v : prerequisites) {
            int a = v[0], b = v[1];
            adj[b].push_back(a);
            indeg[a]++;
        }

        // Courses with no prerequisites are ready to process.
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (indeg[i] == 0) q.push(i);
        }

        vector<int> res;
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            res.push_back(node);

            for (int v : adj[node]) {
                indeg[v]--;
                if (indeg[v] == 0) q.push(v); // All prerequisites for v are complete.
            }
        }

        // Fewer than numCourses nodes processed means the graph contains a cycle.
        if (res.size() != numCourses) return {};
        return res;
    }
};
```