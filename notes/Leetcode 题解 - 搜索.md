<!-- GFM-TOC -->
* [BFS](#bfs)
    * [1. 计算在网格中从原点到特定点的最短路径长度](#1-计算在网格中从原点到特定点的最短路径长度)
    * [2. 组成整数的最小平方数数量](#2-组成整数的最小平方数数量)
    * [3. 最短单词路径](#3-最短单词路径)
* [DFS](#dfs)
    * [1. 查找最大的连通面积](#1-查找最大的连通面积)
    * [2. 矩阵中的连通分量数目](#2-矩阵中的连通分量数目)
    * [3. 好友关系的连通分量数目](#3-好友关系的连通分量数目)
    * [4. 填充封闭区域](#4-填充封闭区域)
    * [5. 能到达的太平洋和大西洋的区域](#5-能到达的太平洋和大西洋的区域)
* [Backtracking](#backtracking)
    * [1. 数字键盘组合](#1-数字键盘组合)
    * [2. IP 地址划分](#2-ip-地址划分)
    * [3. 在矩阵中寻找字符串](#3-在矩阵中寻找字符串)
    * [4. 输出二叉树中所有从根到叶子的路径](#4-输出二叉树中所有从根到叶子的路径)
    * [5. 排列](#5-排列)
    * [6. 含有相同元素求排列](#6-含有相同元素求排列)
    * [7. 组合](#7-组合)
    * [8. 组合求和](#8-组合求和)
    * [9. 含有相同元素的组合求和](#9-含有相同元素的组合求和)
    * [10. 1-9 数字的组合求和](#10-1-9-数字的组合求和)
    * [11. 子集](#11-子集)
    * [12. 含有相同元素求子集](#12-含有相同元素求子集)
    * [13. 分割字符串使得每个部分都是回文数](#13-分割字符串使得每个部分都是回文数)
    * [14. 数独](#14-数独)
    * [15. N 皇后](#15-n-皇后)
<!-- GFM-TOC -->


深度优先搜索和广度优先搜索广泛运用于树和图中，但是它们的应用远远不止如此。

# BFS

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/95903878-725b-4ed9-bded-bc4aae0792a9.jpg"/> </div><br>

广度优先搜索一层一层地进行遍历，每层遍历都是以上一层遍历的结果作为起点，遍历一个距离能访问到的所有节点。需要注意的是，遍历过的节点不能再次被遍历。

第一层：

- 0 -> {6,2,1,5}

第二层：

- 6 -> {4}
- 2 -> {}
- 1 -> {}
- 5 -> {3}

第三层：

- 4 -> {}
- 3 -> {}

每一层遍历的节点都与根节点距离相同。设 d<sub>i</sub> 表示第 i 个节点与根节点的距离，推导出一个结论：对于先遍历的节点 i 与后遍历的节点 j，有 d<sub>i</sub> <= d<sub>j</sub>。利用这个结论，可以求解最短路径等   **最优解**   问题：第一次遍历到目的节点，其所经过的路径为最短路径。应该注意的是，使用 BFS 只能求解无权图的最短路径，无权图是指从一个节点到另一个节点的代价都记为 1。

在程序实现 BFS 时需要考虑以下问题：

- 队列：用来存储每一轮遍历得到的节点；
- 标记：对于遍历过的节点，应该将它标记，防止重复遍历。

## 1. 计算在网格中从原点到特定点的最短路径长度

1091\. Shortest Path in Binary Matrix(Medium)

[Leetcode](https://leetcode.com/problems/shortest-path-in-binary-matrix/) / [力扣](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

```html
[[1,1,0,1],
 [1,0,1,0],
 [1,1,1,1],
 [1,0,1,1]]
```

题目描述：0 表示可以经过某个位置，求解从左上角到右下角的最短路径长度。

```c++
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int steps = 0, n = grid.size();
        queue<pair<int, int>> q1; q1.push({0,0});
        while (!q1.empty()) {
            steps ++;
            queue<pair<int, int>> q2;
            while (!q1.empty()) {
                auto e = q1.front(); q1.pop();
                if (e.first >= 0 && e.first < n && 
                    e.second >= 0 && e.second < n &&
                    grid[e.first][e.second] != 1) {
                    if (e.first == n - 1 && e.second == n - 1) return steps;
                    grid[e.first][e.second] = 1;
                    for (int i = -1; i < 2; ++i)
                        for (int j = -1; j < 2; ++j)
                            if (!(i == 0 && j == 0)) q2.push({e.first + i, e.second + j});
                }
            }
            swap(q1, q2);
        }
        return -1;
    }
};

```

## 2. 组成整数的最小平方数数量

279\. Perfect Squares (Medium)

[Leetcode](https://leetcode.com/problems/perfect-squares/description/) / [力扣](https://leetcode-cn.com/problems/perfect-squares/description/)

```html
For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.
```

本题也可以用动态规划求解，在之后动态规划部分中会再次出现。

```c++
class Solution {
public:
    // DP
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        for (int i = 0; i < n + 1; ++i) {
            if (i < 4) {
                dp[i] = i; continue;
            }
            for (int j = 1; j * j <= i; ++j) {
                dp[i] = min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
     }
    
    // Math
    int numSquares(int n) {
        while (n % 4 == 0) n /= 4;
        if (n % 8 == 7) return 4;
        for (int a = 0; a * a <= n; ++a) {
            int b = sqrt(n - a * a);
            if (a * a + b * b == n) {
                return a > 0 && b > 0 ? 2 : 1;
            }
        }
        return 3;
    }
};
```

## 3. 最短单词路径

127\. Word Ladder (Medium)

[Leetcode](https://leetcode.com/problems/word-ladder/description/) / [力扣](https://leetcode-cn.com/problems/word-ladder/description/)

```html
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.
```

```html
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

题目描述：找出一条从 beginWord 到 endWord 的最短路径，每次移动规定为改变一个字符，并且改变之后的字符串必须在 wordList 中。
kc: 花花酱讲的很好。https://zxi.mytechroad.com/blog/searching/127-word-ladder/

```c++
class Solution {
public:
    // BFS 96 ms
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        if (wordList.empty()) return 0;
        int len = wordList[0].length(), steps = 0;
        unordered_set<string> dict;
        for (string word : wordList)
            dict.insert(word);
        queue<string> q;
        q.push(beginWord);
        while (!q.empty()) {
            steps++;
            int size = q.size();
            for (int j = size; j > 0; --j) {
                string s = q.front(); q.pop();
                for (int i = 0; i < len; ++i) {
                    char tmp = s[i];
                    for (char c = 'a'; c <= 'z'; ++c) {
                        s[i] = c;
                        if (dict.count(s)) {
                            if (s == endWord) return steps + 1;
                            q.push(s);
                            dict.erase(s);
                        }
                    }
                    s[i] = tmp;
                }
            }
        }
        return 0;
    }
        
    // Bi-Directional BFS 44 ms
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        if (wordList.empty()) return 0;
        unordered_set<string> dict(wordList.begin(), wordList.end());
        if (!dict.count(endWord)) return 0;
        int len = wordList[0].length(), steps = 0;
        unordered_set<string> set1{beginWord};
        unordered_set<string> set2{endWord};
        while (!set1.empty() && !set2.empty()) {
            steps++;
            if (set1.size() > set2.size())
                swap(set1, set2);
            unordered_set<string> set;
            for (string s : set1) {
                for (int i = 0; i < len; ++i) {
                    char tmp = s[i];
                    for (char c = 'a'; c <= 'z'; ++c) {
                        s[i] = c;
                        if (set2.count(s)) return steps + 1;
                        if (dict.count(s)) {
                            set.insert(s);
                            dict.erase(s);
                        }
                    }
                    s[i] = tmp;
                }
            }
            swap(set1, set);
        }
        return 0;
    }
};
```

# DFS

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/74dc31eb-6baa-47ea-ab1c-d27a0ca35093.png"/> </div><br>

广度优先搜索一层一层遍历，每一层得到的所有新节点，要用队列存储起来以备下一层遍历的时候再遍历。

而深度优先搜索在得到一个新节点时立即对新节点进行遍历：从节点 0 出发开始遍历，得到到新节点 6 时，立马对新节点 6 进行遍历，得到新节点 4；如此反复以这种方式遍历新节点，直到没有新节点了，此时返回。返回到根节点 0 的情况是，继续对根节点 0 进行遍历，得到新节点 2，然后继续以上步骤。

从一个节点出发，使用 DFS 对一个图进行遍历时，能够遍历到的节点都是从初始节点可达的，DFS 常用来求解这种   **可达性**   问题。

在程序实现 DFS 时需要考虑以下问题：

- 栈：用栈来保存当前节点信息，当遍历新节点返回时能够继续遍历当前节点。可以使用递归栈。
- 标记：和 BFS 一样同样需要对已经遍历过的节点进行标记。

## 1. 查找最大的连通面积

695\. Max Area of Island (Medium)

[Leetcode](https://leetcode.com/problems/max-area-of-island/description/) / [力扣](https://leetcode-cn.com/problems/max-area-of-island/description/)

```html
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int ans = 0;
        vector<pair<int, int>> directions{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int x = 0; x < grid.size(); ++x)
            for (int y = 0; y < grid[0].size(); ++y) {
                int cnt = 0;
                maxAreaOfIsland(grid, x, y, directions, cnt, ans);
            }
        return ans;
    }
private:
    void maxAreaOfIsland(vector<vector<int>>& grid, int x, int y, vector<pair<int, int>>& directions, int& cnt, int& ans) {
        int r = grid.size(), c = grid[0].size();
        if (x == -1 || x == r || y == -1 || y == c || grid[x][y] == 0) return;
        cnt++;
        ans = max(ans, cnt);
        grid[x][y] = 0;
        for (const auto& d : directions)
            maxAreaOfIsland(grid, x + d.first, y + d.second, directions, cnt, ans);
    }
};
```

## 2. 矩阵中的连通分量数目

200\. Number of Islands (Medium)

[Leetcode](https://leetcode.com/problems/number-of-islands/description/) / [力扣](https://leetcode-cn.com/problems/number-of-islands/description/)

```html
Input:
11000
11000
00100
00011

Output: 3
```

可以将矩阵表示看成一张有向图。

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;
        vector<pair<int, int>> directions{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int x = 0; x < grid.size(); ++x)
            for (int y = 0; y < grid[0].size(); ++y) {
                int cnt = 0;
                maxAreaOfIsland(grid, x, y, directions, cnt);
                if (cnt > 0) ans++;
            }
        return ans;
    }
private:
    void maxAreaOfIsland(vector<vector<char>>& grid, int x, int y, vector<pair<int, int>>& directions, int& cnt) {
        int r = grid.size(), c = grid[0].size();
        if (x == -1 || x == r || y == -1 || y == c || grid[x][y] == '0') return;
        cnt++;
        grid[x][y] = '0';
        for (const auto& d : directions)
            maxAreaOfIsland(grid, x + d.first, y + d.second, directions, cnt);
    }
};
```

## 3. 好友关系的连通分量数目

547\. Friend Circles (Medium)

[Leetcode](https://leetcode.com/problems/friend-circles/description/) / [力扣](https://leetcode-cn.com/problems/friend-circles/description/)

```html
Input:
[[1,1,0],
 [1,1,0],
 [0,0,1]]

Output: 2

Explanation:The 0th and 1st students are direct friends, so they are in a friend circle.
The 2nd student himself is in a friend circle. So return 2.
```

题目描述：好友关系可以看成是一个无向图，例如第 0 个人与第 1 个人是好友，那么 M[0][1] 和 M[1][0] 的值都为 1。

```c++
class FindUnion {
public:
    FindUnion (int size) : parent_(size, 0), size_(size, 1), num_of_unions_(size) {
        for (int i = 0; i < size; ++i) {
            parent_[i] = i;
        }
    }
    
    bool Union(int a, int b) {
        int root_a = Find(a);
        int root_b = Find(b);
        if (root_a == root_b) return false;
        if (size_[root_a] > size_[root_b]) {
            parent_[root_b] = root_a;
            size_[root_a] += size_[root_b];
        } else {
            parent_[root_a] = root_b;
            size_[root_b] += size_[root_a];
        }
        num_of_unions_--;
        return true;
    }
    
    int GetNumOfUnions() {
        return num_of_unions_;
    }
      
private:
    int Find(int node) {
        if (node != parent_[node]) {
            parent_[node] = Find(parent_[node]);
        }
        return parent_[node];
    }
    
    vector<int> parent_;
    vector<int> size_;   
    int num_of_unions_;
};

class Solution {
public:
    int findCircleNum(vector<vector<int>>& M) {
        // return Method1(M); //DFS
        return Method3(M); //Union Find
    }

private:
    // DFS
    int Method1(const vector<vector<int>>& M) {
        int ans = 0;
        vector<bool> visited(M.size(), false);
        for (int i = 0; i < M.size(); ++i) {
            if (visited[i]) continue;
            DFS(M, i, visited);
            ans++;
        }
        return ans;
    }
    
    void DFS(const vector<vector<int>>& M, int curr, vector<bool>& visited) {
        for (int i = 0; i < M.size(); ++i) {
            if (visited[i]) continue;
            if (M[curr][i]) {
                visited[i] = true;
                DFS(M, i, visited);
            }
        }
    }
    
    // union find
    int Method2(vector<vector<int>>& M) {
        FindUnion fu(M.size());
        for (int i = 0; i < M.size(); ++i) {
            for (int j = 0; j < M[0].size(); ++j) {
                if (M[i][j]) fu.Union(i, j);
            }
        }
        return fu.GetNumOfUnions();
    }
    
    // union find - 2nd time by myself
    int Method3(vector<vector<int>>& M) {
        vector<int> root(M.size());
        for (int i = 0; i < M.size(); ++i)
            root[i] = i;
        
        for (int i = 0; i < M.size(); ++i) {
            for (int j = 0; j < M[0].size(); ++j) {
                if (M[i][j]) {
                    int root_i = GetRoot(i, root);
                    int root_j = GetRoot(j, root);
                    root[root_i] = root_j;
                }
            }
        }
        unordered_set<int> circles;
        for (int i = 0; i < M.size(); ++i)
            circles.insert(GetRoot(i, root));
        return circles.size();
    }
    
    int GetRoot(int stu, vector<int>& root) {
        if (root[stu] != stu) {
            root[stu] = GetRoot(root[stu], root);
        }
        return root[stu];
    }
};
```

## 4. 填充封闭区域

130\. Surrounded Regions (Medium)

[Leetcode](https://leetcode.com/problems/surrounded-regions/description/) / [力扣](https://leetcode-cn.com/problems/surrounded-regions/description/)

```html
For example,
X X X X
X O O X
X X O X
X O X X

After running your function, the board should be:
X X X X
X X X X
X X X X
X O X X
```

题目描述：使被 'X' 包围的 'O' 转换为 'X'。

```c++
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.size() < 3 || board[0].size() < 3)
            return;
        vector<pair<int, int>> directions{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int r = board.size(), c = board[0].size();
        for (int i = 0; i < r; ++i) {
            DFS(board, i, 0, directions);
            DFS(board, i, c - 1, directions);
        }
        for (int j = 0; j < c; ++j) {
            DFS(board, 0, j, directions);
            DFS(board, r - 1, j, directions);
        }
        for (int i = 0; i < r; ++i)
            for (int j = 0; j < c; ++j) {
                if (board[i][j] == 'O') board[i][j] = 'X';
                if (board[i][j] == 'T') board[i][j] = 'O';
        }
    }
private:
    void DFS(vector<vector<char>>& board, int i, int j, vector<pair<int, int>>& directions) {
        if (i == -1 || i == board.size() || j == -1 || j == board[0].size() || 
            board[i][j] == 'X' || board[i][j] == 'T') return;
        board[i][j] = 'T';
        for (const auto& p : directions)
            DFS(board, i + p.first, j + p.second, directions);
    }
};
```

## 5. 能到达的太平洋和大西洋的区域

417\. Pacific Atlantic Water Flow (Medium)

[Leetcode](https://leetcode.com/problems/pacific-atlantic-water-flow/description/) / [力扣](https://leetcode-cn.com/problems/pacific-atlantic-water-flow/description/)

```html
Given the following 5x5 matrix:

  Pacific ~   ~   ~   ~   ~
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * Atlantic

Return:
[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (positions with parentheses in above matrix).
```

左边和上边是太平洋，右边和下边是大西洋，内部的数字代表海拔，海拔高的地方的水能够流到低的地方，求解水能够流到太平洋和大西洋的所有位置。

```c++
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& matrix) {
        if (matrix.empty()) return {};
        const int r = matrix.size(), c = matrix[0].size();

        vector<vector<int>> p(r, vector<int>(c));
        vector<vector<int>> a(r, vector<int>(c));
        vector<pair<int, int>> directions{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        for (int x = 0; x < r; ++x) {
            DFS(matrix, x, 0, 0, p, directions);  // left
            DFS(matrix, x, c - 1, 0, a, directions); // right
        }
    
        for (int y = 0; y < c; ++y) {
            DFS(matrix, 0, y, 0, p, directions);  // top
            DFS(matrix, r - 1, y, 0, a, directions); // bottom
        }  
        
        vector<vector<int>> ans;
        for (int i = 0; i < r; ++i)
            for (int j = 0; j < c; ++j)
                if (p[i][j] && a[i][j]) ans.push_back({i, j});
        return ans;
    }
private:
    void DFS(vector<vector<int>>& h, int x, int y, int pre_h, vector<vector<int>>& ocean, vector<pair<int, int>>& directions) {
        if (x == -1 || y == -1) return;
        if (x == h.size() || y == h[0].size()) return;
        if (h[x][y] < pre_h) return;
        if (ocean[x][y] == 1) return;
        ocean[x][y] = 1;       
        for (const auto& p : directions)
            DFS(h, x + p.first, y + p.second, h[x][y], ocean, directions);
    }
};
```

# Backtracking

Backtracking（回溯）属于 DFS。

- 普通 DFS 主要用在   **可达性问题**  ，这种问题只需要执行到特点的位置然后返回即可。
- 而 Backtracking 主要用于求解   **排列组合**   问题，例如有 { 'a','b','c' } 三个字符，求解所有由这三个字符排列得到的字符串，这种问题在执行到特定的位置返回之后还会继续执行求解过程。

因为 Backtracking 不是立即返回，而要继续求解，因此在程序实现时，需要注意对元素的标记问题：

- 在访问一个新元素进入新的递归调用时，需要将新元素标记为已经访问，这样才能在继续递归调用时不用重复访问该元素；
- 但是在递归返回时，需要将元素标记为未访问，因为只需要保证在一个递归链中不同时访问一个元素，可以访问已经访问过但是不在当前递归链中的元素。

## 1. 数字键盘组合

17\. Letter Combinations of a Phone Number (Medium)

[Leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) / [力扣](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9823768c-212b-4b1a-b69a-b3f59e07b977.jpg"/> </div><br>

```html
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) return {};
        vector<string> dict{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> ans;
        string cur;
        DFS(digits, 0, dict, cur, ans);
        return ans;
    }
    
    void DFS (string& digits, int len, vector<string>& dict, string cur, vector<string>& ans) {
        if (len == digits.size()) {
            ans.push_back(cur);
            return;
        }
        for (const auto c : dict[digits.at(len) - '0']) 
            DFS(digits, len + 1, dict, cur + c, ans);
    }
};
```

## 2. IP 地址划分

93\. Restore IP Addresses(Medium)

[Leetcode](https://leetcode.com/problems/restore-ip-addresses/description/) / [力扣](https://leetcode-cn.com/problems/restore-ip-addresses/description/)

```html
Given "25525511135",
return ["255.255.11.135", "255.255.111.35"].
```

```c++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        if (s.empty()) return {};
        vector<string> ans;
        vector<string> sections;
        DFS(s, 0, sections, ans);
        return ans;
    }
    
    void DFS(string s, int l, vector<string>& sections, vector<string>& ans) {
        if (sections.size() > 0 && !Valid(sections.back()))
            return;        
        if (sections.size() == 4 && l < s.size())
            return;        
        if (sections.size() == 4 && l == s.size()) {
            ans.push_back(sections[0] + "." + sections[1] + "." + 
                          sections[2] + "." + sections[3]);
            return;
        }
        for (int len = 1; len <= 3; ++len) {
            if (l + len > s.size()) continue;
            sections.push_back(s.substr(l, len));
            DFS(s, l + len, sections, ans);
            sections.pop_back();
        }
    }
    
    bool Valid(string s) {
        if (s.empty()) return false;
        if (s.size() == 1) return true;
        if (s.size() == 2) return s[0] != '0';
        if (s.size() == 3) return s[0] != '0' && stoi(s) <= 255;
        return false;
    }
};
```

## 3. 在矩阵中寻找字符串

79\. Word Search (Medium)

[Leetcode](https://leetcode.com/problems/word-search/description/) / [力扣](https://leetcode-cn.com/problems/word-search/description/)

```html
For example,
Given board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.
```

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty() || board[0].empty() || word.empty()) return false;
        for (int i = 0; i < board.size(); ++i)
            for (int j = 0; j < board[0].size(); ++j) {
                if (board[i][j] == word[0] && DFS(board, word, i, j))
                    return true;
            }
        return false;
    }
    
    bool DFS(vector<vector<char>>& board, string word, int x, int y) {
        if (x < 0 || y < 0 || x == board.size() || y == board[0].size()) return false;
        if (word.empty() || board[x][y] != word[0]) return false;
        if (word.size() == 1 && board[x][y] == word[0]) return true;
        bool ans = false;
        vector<int> d{0, 1, 0, -1, 0};
        char tmp = board[x][y];
        board[x][y] = '0';
        // Time Limit Exceeded: (why?)
        // for (int i = 0; i < d.size() - 1; ++i)
        //     ans |= DFS(board, word.substr(1), x + d[i], y + d[i + 1]);

        ans = DFS(board, word.substr(1), x + 1, y) ||
              DFS(board, word.substr(1), x - 1, y) ||
              DFS(board, word.substr(1), x, y + 1) ||
              DFS(board, word.substr(1), x, y - 1);
        board[x][y] = tmp;
        return ans;
    }
};
```

## 4. 输出二叉树中所有从根到叶子的路径

257\. Binary Tree Paths (Easy)

[Leetcode](https://leetcode.com/problems/binary-tree-paths/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-paths/description/)

```html
  1
 /  \
2    3
 \
  5
```

```html
["1->2->5", "1->3"]
```

```c++
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ans;
        DFS(root, "", ans);
        return ans;
    }
    
    void DFS(TreeNode* root, string cur, vector<string>& ans) {
        if (root == nullptr) return;
        string s = cur.empty() ? to_string(root->val) : "->" + to_string(root->val);
        if (root->left == nullptr && root->right == nullptr) {
            ans.push_back(cur + s);
            return;
        } 
        DFS(root->left, cur + s, ans);
        DFS(root->right, cur + s, ans);
    }
};
```

## 5. 排列

46\. Permutations (Medium)

[Leetcode](https://leetcode.com/problems/permutations/description/) / [力扣](https://leetcode-cn.com/problems/permutations/description/)

```html
[1,2,3] have the following permutations:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        if (nums.empty()) return {{}};
        vector<int> visited(nums.size(), 0);
        vector<vector<int>> ans;
        vector<int> cur;
        DFS(nums, visited, cur, ans);
        return ans;
    }
    
    void DFS(vector<int>& nums, vector<int>& visited, vector<int>& cur, vector<vector<int>>& ans) {
        if (cur.size() == nums.size()) {
            ans.push_back(cur); return;
        }
        for (int i = 0; i < nums.size(); ++i) {
            if (visited[i]) continue;
            visited[i] = 1;
            cur.push_back(nums[i]);
            DFS(nums, visited, cur, ans);
            cur.pop_back();
            visited[i] = 0;
        }
    }
};
```

## 6. 含有相同元素求排列

47\. Permutations II (Medium)

[Leetcode](https://leetcode.com/problems/permutations-ii/description/) / [力扣](https://leetcode-cn.com/problems/permutations-ii/description/)

```html
[1,1,2] have the following unique permutations:
[[1,1,2], [1,2,1], [2,1,1]]
```

数组元素可能含有相同的元素，进行排列时就有可能出现重复的排列，要求重复的排列只返回一个。

在实现上，和 Permutations 不同的是要先排序，然后在添加一个元素时，判断这个元素是否等于前一个元素，如果等于，并且前一个元素还未访问，那么就跳过这个元素。

```c++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        if (nums.empty()) return {{}};
        vector<int> visited(nums.size(), 0);
        vector<vector<int>> ans;
        vector<int> cur;
        sort(nums.begin(), nums.end());
        DFS(nums, visited, cur, ans);
        return ans;
    }
    
    void DFS(vector<int>& nums, vector<int>& visited, vector<int>& cur, vector<vector<int>>& ans) {
        if (cur.size() == nums.size()) {
            ans.push_back(cur); return;
        }
        for (int i = 0; i < nums.size(); ++i) {
            if (visited[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;
            visited[i] = 1;
            cur.push_back(nums[i]);
            DFS(nums, visited, cur, ans);
            cur.pop_back();
            visited[i] = 0;
        }
    }
};
```

## 7. 组合

77\. Combinations (Medium)

[Leetcode](https://leetcode.com/problems/combinations/description/) / [力扣](https://leetcode-cn.com/problems/combinations/description/)

```html
If n = 4 and k = 2, a solution is:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> combinations = new ArrayList<>();
    List<Integer> combineList = new ArrayList<>();
    backtracking(combineList, combinations, 1, k, n);
    return combinations;
}

private void backtracking(List<Integer> combineList, List<List<Integer>> combinations, int start, int k, final int n) {
    if (k == 0) {
        combinations.add(new ArrayList<>(combineList));
        return;
    }
    for (int i = start; i <= n - k + 1; i++) {  // 剪枝
        combineList.add(i);
        backtracking(combineList, combinations, i + 1, k - 1, n);
        combineList.remove(combineList.size() - 1);
    }
}
```

## 8. 组合求和

39\. Combination Sum (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum/description/)

```html
given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[[7],[2, 2, 3]]
```

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> combinations = new ArrayList<>();
    backtracking(new ArrayList<>(), combinations, 0, target, candidates);
    return combinations;
}

private void backtracking(List<Integer> tempCombination, List<List<Integer>> combinations,
                          int start, int target, final int[] candidates) {

    if (target == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        if (candidates[i] <= target) {
            tempCombination.add(candidates[i]);
            backtracking(tempCombination, combinations, i, target - candidates[i], candidates);
            tempCombination.remove(tempCombination.size() - 1);
        }
    }
}
```

## 9. 含有相同元素的组合求和

40\. Combination Sum II (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-ii/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-ii/description/)

```html
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> combinations = new ArrayList<>();
    Arrays.sort(candidates);
    backtracking(new ArrayList<>(), combinations, new boolean[candidates.length], 0, target, candidates);
    return combinations;
}

private void backtracking(List<Integer> tempCombination, List<List<Integer>> combinations,
                          boolean[] hasVisited, int start, int target, final int[] candidates) {

    if (target == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    for (int i = start; i < candidates.length; i++) {
        if (i != 0 && candidates[i] == candidates[i - 1] && !hasVisited[i - 1]) {
            continue;
        }
        if (candidates[i] <= target) {
            tempCombination.add(candidates[i]);
            hasVisited[i] = true;
            backtracking(tempCombination, combinations, hasVisited, i + 1, target - candidates[i], candidates);
            hasVisited[i] = false;
            tempCombination.remove(tempCombination.size() - 1);
        }
    }
}
```

## 10. 1-9 数字的组合求和

216\. Combination Sum III (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-iii/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-iii/description/)

```html
Input: k = 3, n = 9

Output:

[[1,2,6], [1,3,5], [2,3,4]]
```

从 1-9 数字中选出 k 个数不重复的数，使得它们的和为 n。

```java
public List<List<Integer>> combinationSum3(int k, int n) {
    List<List<Integer>> combinations = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    backtracking(k, n, 1, path, combinations);
    return combinations;
}

private void backtracking(int k, int n, int start,
                          List<Integer> tempCombination, List<List<Integer>> combinations) {

    if (k == 0 && n == 0) {
        combinations.add(new ArrayList<>(tempCombination));
        return;
    }
    if (k == 0 || n == 0) {
        return;
    }
    for (int i = start; i <= 9; i++) {
        tempCombination.add(i);
        backtracking(k - 1, n - i, i + 1, tempCombination, combinations);
        tempCombination.remove(tempCombination.size() - 1);
    }
}
```

## 11. 子集

78\. Subsets (Medium)

[Leetcode](https://leetcode.com/problems/subsets/description/) / [力扣](https://leetcode-cn.com/problems/subsets/description/)

找出集合的所有子集，子集不能重复，[1, 2] 和 [2, 1] 这种子集算重复

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> subsets = new ArrayList<>();
    List<Integer> tempSubset = new ArrayList<>();
    for (int size = 0; size <= nums.length; size++) {
        backtracking(0, tempSubset, subsets, size, nums); // 不同的子集大小
    }
    return subsets;
}

private void backtracking(int start, List<Integer> tempSubset, List<List<Integer>> subsets,
                          final int size, final int[] nums) {

    if (tempSubset.size() == size) {
        subsets.add(new ArrayList<>(tempSubset));
        return;
    }
    for (int i = start; i < nums.length; i++) {
        tempSubset.add(nums[i]);
        backtracking(i + 1, tempSubset, subsets, size, nums);
        tempSubset.remove(tempSubset.size() - 1);
    }
}
```

## 12. 含有相同元素求子集

90\. Subsets II (Medium)

[Leetcode](https://leetcode.com/problems/subsets-ii/description/) / [力扣](https://leetcode-cn.com/problems/subsets-ii/description/)

```html
For example,
If nums = [1,2,2], a solution is:

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> subsets = new ArrayList<>();
    List<Integer> tempSubset = new ArrayList<>();
    boolean[] hasVisited = new boolean[nums.length];
    for (int size = 0; size <= nums.length; size++) {
        backtracking(0, tempSubset, subsets, hasVisited, size, nums); // 不同的子集大小
    }
    return subsets;
}

private void backtracking(int start, List<Integer> tempSubset, List<List<Integer>> subsets, boolean[] hasVisited,
                          final int size, final int[] nums) {

    if (tempSubset.size() == size) {
        subsets.add(new ArrayList<>(tempSubset));
        return;
    }
    for (int i = start; i < nums.length; i++) {
        if (i != 0 && nums[i] == nums[i - 1] && !hasVisited[i - 1]) {
            continue;
        }
        tempSubset.add(nums[i]);
        hasVisited[i] = true;
        backtracking(i + 1, tempSubset, subsets, hasVisited, size, nums);
        hasVisited[i] = false;
        tempSubset.remove(tempSubset.size() - 1);
    }
}
```

## 13. 分割字符串使得每个部分都是回文数

131\. Palindrome Partitioning (Medium)

[Leetcode](https://leetcode.com/problems/palindrome-partitioning/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-partitioning/description/)

```html
For example, given s = "aab",
Return

[
  ["aa","b"],
  ["a","a","b"]
]
```

```java
public List<List<String>> partition(String s) {
    List<List<String>> partitions = new ArrayList<>();
    List<String> tempPartition = new ArrayList<>();
    doPartition(s, partitions, tempPartition);
    return partitions;
}

private void doPartition(String s, List<List<String>> partitions, List<String> tempPartition) {
    if (s.length() == 0) {
        partitions.add(new ArrayList<>(tempPartition));
        return;
    }
    for (int i = 0; i < s.length(); i++) {
        if (isPalindrome(s, 0, i)) {
            tempPartition.add(s.substring(0, i + 1));
            doPartition(s.substring(i + 1), partitions, tempPartition);
            tempPartition.remove(tempPartition.size() - 1);
        }
    }
}

private boolean isPalindrome(String s, int begin, int end) {
    while (begin < end) {
        if (s.charAt(begin++) != s.charAt(end--)) {
            return false;
        }
    }
    return true;
}
```

## 14. 数独

37\. Sudoku Solver (Hard)

[Leetcode](https://leetcode.com/problems/sudoku-solver/description/) / [力扣](https://leetcode-cn.com/problems/sudoku-solver/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/0e8fdc96-83c1-4798-9abe-45fc91d70b9d.png"/> </div><br>

```java
private boolean[][] rowsUsed = new boolean[9][10];
private boolean[][] colsUsed = new boolean[9][10];
private boolean[][] cubesUsed = new boolean[9][10];
private char[][] board;

public void solveSudoku(char[][] board) {
    this.board = board;
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == '.') {
                continue;
            }
            int num = board[i][j] - '0';
            rowsUsed[i][num] = true;
            colsUsed[j][num] = true;
            cubesUsed[cubeNum(i, j)][num] = true;
        }
        backtracking(0, 0);
}

private boolean backtracking(int row, int col) {
    while (row < 9 && board[row][col] != '.') {
        row = col == 8 ? row + 1 : row;
        col = col == 8 ? 0 : col + 1;
    }
    if (row == 9) {
        return true;
    }
    for (int num = 1; num <= 9; num++) {
        if (rowsUsed[row][num] || colsUsed[col][num] || cubesUsed[cubeNum(row, col)][num]) {
            continue;
        }
        rowsUsed[row][num] = colsUsed[col][num] = cubesUsed[cubeNum(row, col)][num] = true;
        board[row][col] = (char) (num + '0');
        if (backtracking(row, col)) {
            return true;
        }
        board[row][col] = '.';
        rowsUsed[row][num] = colsUsed[col][num] = cubesUsed[cubeNum(row, col)][num] = false;
    }
    return false;
}

private int cubeNum(int i, int j) {
    int r = i / 3;
    int c = j / 3;
    return r * 3 + c;
}
```

## 15. N 皇后

51\. N-Queens (Hard)

[Leetcode](https://leetcode.com/problems/n-queens/description/) / [力扣](https://leetcode-cn.com/problems/n-queens/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/067b310c-6877-40fe-9dcf-10654e737485.jpg"/> </div><br>

在 n\*n 的矩阵中摆放 n 个皇后，并且每个皇后不能在同一行，同一列，同一对角线上，求所有的 n 皇后的解。

一行一行地摆放，在确定一行中的那个皇后应该摆在哪一列时，需要用三个标记数组来确定某一列是否合法，这三个标记数组分别为：列标记数组、45 度对角线标记数组和 135 度对角线标记数组。

45 度对角线标记数组的长度为 2 \* n - 1，通过下图可以明确 (r, c) 的位置所在的数组下标为 r + c。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9c422923-1447-4a3b-a4e1-97e663738187.jpg" width="300px"> </div><br>


135 度对角线标记数组的长度也是 2 \* n - 1，(r, c) 的位置所在的数组下标为 n - 1 - (r - c)。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/7a85e285-e152-4116-b6dc-3fab27ba9437.jpg" width="300px"> </div><br>

```java
private List<List<String>> solutions;
private char[][] nQueens;
private boolean[] colUsed;
private boolean[] diagonals45Used;
private boolean[] diagonals135Used;
private int n;

public List<List<String>> solveNQueens(int n) {
    solutions = new ArrayList<>();
    nQueens = new char[n][n];
    for (int i = 0; i < n; i++) {
        Arrays.fill(nQueens[i], '.');
    }
    colUsed = new boolean[n];
    diagonals45Used = new boolean[2 * n - 1];
    diagonals135Used = new boolean[2 * n - 1];
    this.n = n;
    backtracking(0);
    return solutions;
}

private void backtracking(int row) {
    if (row == n) {
        List<String> list = new ArrayList<>();
        for (char[] chars : nQueens) {
            list.add(new String(chars));
        }
        solutions.add(list);
        return;
    }

    for (int col = 0; col < n; col++) {
        int diagonals45Idx = row + col;
        int diagonals135Idx = n - 1 - (row - col);
        if (colUsed[col] || diagonals45Used[diagonals45Idx] || diagonals135Used[diagonals135Idx]) {
            continue;
        }
        nQueens[row][col] = 'Q';
        colUsed[col] = diagonals45Used[diagonals45Idx] = diagonals135Used[diagonals135Idx] = true;
        backtracking(row + 1);
        colUsed[col] = diagonals45Used[diagonals45Idx] = diagonals135Used[diagonals135Idx] = false;
        nQueens[row][col] = '.';
    }
}
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
