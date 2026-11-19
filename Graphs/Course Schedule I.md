**Question**: [Course Schedule](https://leetcode.com/problems/course-schedule/)

**Approach**:
- Model courses as graph nodes and each prerequisite pair as a directed edge from the prerequisite course to the dependent course.
- Build the adjacency list and count the indegree of every course.
- Use Kahn's algorithm: initially queue every course with indegree zero, then remove courses one by one and decrement the indegrees of their dependents.
- When a dependent's indegree reaches zero, add it to the queue. If all courses are processed, there is no directed cycle and the courses can all be completed.

TC: `O(V + E)`, where $V = numCourses$ and $E$ is the number of prerequisite pairs.
SC: `O(V + E)` for the adjacency list and indegree array; the queue uses up to `O(V)` auxiliary space.

**Code**:
```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> indeg(numCourses, 0);
        vector<vector<int>> adj(numCourses);

        // Each pair [a, b] means b must be completed before a.
        for (const vector<int>& pre : prerequisites) {
            int a = pre[0], b = pre[1];
            adj[b].push_back(a);
            indeg[a]++;
        }

        // Courses with no prerequisites are ready to process.
        queue<int> q;
        for (int i = 0; i < numCourses; i++) {
            if (indeg[i] == 0) q.push(i);
        }

        int res = 0; // Number of courses processed in topological order.
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            res++;

            for (int v : adj[node]) {
                indeg[v]--;
                if (indeg[v] == 0) q.push(v); // All prerequisites for v are complete.
            }
        }

        // A cycle prevents every course from being processed.
        return res == numCourses;
    }
};
```