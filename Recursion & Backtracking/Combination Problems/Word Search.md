**Question**: [Word Search](https://leetcode.com/problems/word-search/)

**Approach**:

- Try every board cell as the possible starting position for the first character of `word`.
- At a cell, verify that the board character matches `word[idx]` and that the cell is not already used in the current path.
- Mark the matching cell as visited, then recursively try the four horizontal and vertical neighbors for the next character.
- If the current index is the final character, a complete match has been found. Otherwise, unmark the cell after all branches fail so it can participate in a different path.

TC: $O(n \cdot m \cdot 4^L)$, where $n \times m$ is the board size and $L$ is `word.size()`. The bound comes from at most four directions at each position of the DFS.

SC: $O(n \cdot m + L)$ for the visited matrix and recursion stack. The board dimensions and path length determine the auxiliary space.

**Code**:
```cpp
class Solution {
public:
    bool solve(const vector<vector<char>>& board, const string& word,
               int idx, pair<int, int> position,
               vector<vector<int>>& vis) {
        int i = position.first;
        int j = position.second;

        // The current board character must match the next character of word.
        if (word[idx] != board[i][j]) return false;

        // Each cell may be used only once within the current search path.
        if (vis[i][j]) return false;

        // The entire word has been matched.
        if (idx == (int)word.size() - 1) return true;

        vis[i][j] = 1;

        static const int dx[4] = {0, 0, 1, -1};
        static const int dy[4] = {1, -1, 0, 0};

        for (int dir = 0; dir < 4; dir++) {
            int nextI = i + dx[dir];
            int nextJ = j + dy[dir];

            // Skip neighbors outside the board.
            if (nextI < 0 || nextI >= board.size() ||
                nextJ < 0 || nextJ >= board[0].size()) {
                continue;
            }

            if (solve(board, word, idx + 1, {nextI, nextJ}, vis)) {
                return true;
            }
        }

        // This cell did not lead to a match for this path, so make it reusable.
        vis[i][j] = 0;
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size();
        int m = board[0].size();
        vector<vector<int>> vis(n, vector<int>(m, 0));

        // Try each board cell as the beginning of the word.
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (solve(board, word, 0, {i, j}, vis)) {
                    return true;
                }
            }
        }

        return false;
    }
};
```
