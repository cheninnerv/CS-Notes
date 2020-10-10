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

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> comb;
        DFS(n, k, 1, comb, ans);
        return ans;
    }
    
    void DFS(int n, int k, int s, vector<int>& comb, vector<vector<int>>& ans) {
        if (comb.size() == k) {
            ans.push_back(comb);
            return;
        }
        for (int i = s; i <= n; ++i) {
            comb.push_back(i);
            DFS(n, k, i + 1, comb, ans);
            comb.pop_back();
        }
    }
};
```

## 8. 组合求和

39\. Combination Sum (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum/description/)

```html
given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[[7],[2, 2, 3]]
```

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> cand;
        DFS(candidates, 0, cand, 0, target, ans);
        return ans;
    }
    
    void DFS(vector<int>& candidates, int start, vector<int>& cand, int sum, 
             int target, vector<vector<int>>& ans) {
        if (sum == target) {
            ans.push_back(cand);
            return;
        }
        if (sum > target) return;
        for (int i = start; i < candidates.size(); ++i) {
            cand.push_back(candidates[i]);
            DFS(candidates, i, cand, sum + candidates[i], target, ans);
            cand.pop_back();
        }
    }
};
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

```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> cand;
        vector<int> visited(candidates.size());
        sort(candidates.begin(), candidates.end());
        DFS(candidates, 0, cand, 0, target, visited, ans);
        return ans;
    }
    
    void DFS(vector<int>& candidates, int start, vector<int>& cand, int sum, 
             int target, vector<int>& visited, vector<vector<int>>& ans) {
        if (sum == target) {
            ans.push_back(cand);
            return;
        }
        if (sum > target) return;
        for (int i = start; i < candidates.size(); ++i) {
            if (i > 0 && candidates[i] == candidates[i - 1] && !visited[i - 1]) continue;
            visited[i] = 1;
            cand.push_back(candidates[i]);
            DFS(candidates, i + 1, cand, sum + candidates[i], target, visited, ans);
            cand.pop_back();
            visited[i] = 0;
        }
    }
};
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

```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        vector<int> cand;
        DFS(k, 1, cand, 0, n, ans);
        return ans;
    }
    
    void DFS(const int& k, int start, vector<int>& cand, int sum, 
             int target, vector<vector<int>>& ans) {
        if (cand.size() == k && sum == target) {
            ans.push_back(cand);
            return;
        }
        if (sum > target) return;
        for (int i = start; i <= 9; ++i) {
            cand.push_back(i);
            DFS(k, i + 1, cand, sum + i, target, ans);
            cand.pop_back();
        }
    }
};
```

## 11. 子集

78\. Subsets (Medium)

[Leetcode](https://leetcode.com/problems/subsets/description/) / [力扣](https://leetcode-cn.com/problems/subsets/description/)

找出集合的所有子集，子集不能重复，[1, 2] 和 [2, 1] 这种子集算重复

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> cand;
        for (int k = 0; k <= nums.size(); ++k) {
            DFS(nums, 0, cand, k, ans);
        }
        return ans;
    }
    
    void DFS(vector<int>& nums, int start, vector<int>& cand, int k, vector<vector<int>>& ans) {
        if (cand.size() == k) {
            ans.push_back(cand);
            return;
        }
        for (int i = start; i < nums.size(); ++i) {
            cand.push_back(nums[i]);
            DFS(nums, i + 1, cand, k, ans);
            cand.pop_back();
        }
    }
};
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

```c++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> cand;
        sort(nums.begin(), nums.end());
        vector<int> visited(nums.size());
        for (int k = 0; k <= nums.size(); ++k) {
            DFS(nums, 0, cand, visited, k, ans);
        }
        return ans;
    }
    
    void DFS(vector<int>& nums, int start, vector<int>& cand, vector<int>& visited, int k, vector<vector<int>>& ans) {
        if (cand.size() == k) {
            ans.push_back(cand);
            return;
        }
        for (int i = start; i < nums.size(); ++i) {
            if (i > 0 && nums[i] == nums[i - 1] && !visited[i - 1]) continue;
            visited[i] = 1;
            cand.push_back(nums[i]);
            DFS(nums, i + 1, cand, visited, k, ans);
            cand.pop_back();
            visited[i] = 0;
        }
    }
};
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

```c++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> ans;
        vector<string> row;
        PartitionHelper(s, 0, row, ans);
        return ans;
    }
private:
    void PartitionHelper(string s, int start, vector<string>& row, vector<vector<string>>& ans) {
        if(start == s.length()) {
            ans.push_back(row);
            return;
        }
        for (int i = start; i < s.length(); ++i) {
            if (!IsPalindrome(s, start, i))
                continue;
            row.push_back(s.substr(start, i - start + 1));
            PartitionHelper(s, i + 1, row, ans);
            row.pop_back();
        }
    }
     
    bool IsPalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end]) return false;
            ++start; --end;
        }
        return true;
    }
};
```

## 14. 数独

37\. Sudoku Solver (Hard)

[Leetcode](https://leetcode.com/problems/sudoku-solver/description/) / [力扣](https://leetcode-cn.com/problems/sudoku-solver/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/0e8fdc96-83c1-4798-9abe-45fc91d70b9d.png"/> </div><br>
kc:https://zxi.mytechroad.com/blog/searching/leetcode-37-sudoku-solver/
遍历一遍matrix，建3个分别查询行，列，格，valid与否的数组（2d)
 vector<vector<int>> rows_ 代表第几行（0-8）有没有数字X。 cols_代表第几列（0-8）有没有数字X， boxes_代表第几个格子（0-8）有没有数字X。另一个比较巧妙的写法是不写循环，而是用next_x, next_y来遍历，因为是个9x9的棋盘所以这两个值都很容易算，终止条件就是x==9 （0-8）。 一个出错的点：help function用了void没有bool做返回值，中间没有if这个判断if (solveSudoku(board, next_i, next_j)) return true;程序会在返回过程中总会继续跑下面的语句比如board[i][j] = '.';等，导致所有填好的val又全部清回'.'。
   
```c++
class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        rows_ = vector<vector<int>>(9, vector<int>(10));
        cols_ = vector<vector<int>>(9, vector<int>(10));
        grids_ = vector<vector<int>>(9, vector<int>(10));
        
        for (int i = 0; i < board.size(); ++i)
            for (int j = 0; j < board[0].size(); ++j) {                
                if (board[i][j] != '.') {
                    int val = board[i][j] - '0';
                    rows_[i][val] = 1;
                    cols_[j][val] = 1;
                    grids_[(i / 3) * 3 + (j / 3)][val] = 1;
                }
            }
        
        solveSudoku(board, 0, 0);
    }
private:
    bool solveSudoku(vector<vector<char>>& board, int i, int j) {
        if (i == 9) return true;
        int next_j = (j + 1) % 9;
        int next_i = next_j == 0 ? i + 1: i;
        if (board[i][j] != '.')
            return solveSudoku(board, next_i, next_j);
        for (int val = 1; val <= 9; ++val) {
            if (!rows_[i][val] && !cols_[j][val] && !grids_[(i / 3) * 3 + (j / 3)][val]) {
                rows_[i][val] = 1;
                cols_[j][val] = 1;
                grids_[(i / 3) * 3 + (j / 3)][val] = 1;
                board[i][j] = val + '0';
                if (solveSudoku(board, next_i, next_j)) return true;
                board[i][j] = '.';
                rows_[i][val] = 0;
                cols_[j][val] = 0;
                grids_[(i / 3) * 3 + (j / 3)][val] = 0;            
            }
        }
        return false;
    }
    
    vector<vector<int>> rows_;
    vector<vector<int>> cols_;
    vector<vector<int>> grids_;
};
```

## 15. N 皇后

51\. N-Queens (Hard)

[Leetcode](https://leetcode.com/problems/n-queens/description/) / [力扣](https://leetcode-cn.com/problems/n-queens/description/)

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/067b310c-6877-40fe-9dcf-10654e737485.jpg"/> </div><br>

在 n\*n 的矩阵中摆放 n 个皇后，并且每个皇后不能在同一行，同一列，同一对角线上，求所有的 n 皇后的解。

一行一行地摆放，在确定一行中的那个皇后应该摆在哪一列时，需要用三个标记数组来确定某一位置[i(row)，j(column)]是否合法，这三个标记数组分别为：列标记数组cols_[j]、45 度对角线标记数组diagonal1_[i + j]和 135 度对角线标记数组diagonal2_[j - i + n - 1], 行不需要了因为是一行行往下走，所以保证一行只有一个Queen。
https://zxi.mytechroad.com/blog/searching/leetcode-51-n-queens/

```c++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        if (n < 1) return {{}};
        cols_ = vector<int>(n);
        diagonal1_ = vector<int>(n * 2 - 1);
        diagonal2_ = vector<int>(n * 2 - 1);
        vector<vector<string>> ans;
        vector<string> rows;
        solveNQueens(n, 0, rows, ans);
        return ans;
    }
private:
    void solveNQueens(int n, int i, vector<string>& rows, vector<vector<string>>& ans) {
        if (i == n) {
            ans.push_back(rows); return;
        }
        for (int j = 0; j < n; ++j) {
            if (!cols_[j] && !diagonal1_[i + j] && !diagonal2_[j - i + n - 1]) {
                cols_[j] = 1;
                diagonal1_[i + j] = 1;
                diagonal2_[j - i + n - 1] = 1;
                rows.push_back(CreateRow(n, j));
                solveNQueens(n, i + 1, rows, ans);
                rows.pop_back();
                cols_[j] = 0;
                diagonal1_[i + j] = 0;
                diagonal2_[j - i + n - 1] = 0;
            }
        }
    }
    
    string CreateRow(int n, int j) {
        string s;
        for (int i = 0; i < n; ++i) {
            if (i == j) s.push_back('Q');
            else s.push_back('.');
        }
        return s;
    }
    
    vector<int> cols_;
    vector<int> rows_;
    vector<int> diagonal1_;
    vector<int> diagonal2_;
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
