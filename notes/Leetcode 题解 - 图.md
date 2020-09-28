<!-- GFM-TOC -->
* [二分图](#二分图)
    * [1. 判断是否为二分图](#1-判断是否为二分图)
* [拓扑排序](#拓扑排序)
    * [1. 课程安排的合法性](#1-课程安排的合法性)
    * [2. 课程安排的顺序](#2-课程安排的顺序)
* [并查集](#并查集)
    * [1. 冗余连接](#1-冗余连接)
<!-- GFM-TOC -->


# 二分图

如果可以用两种颜色对图中的节点进行着色，并且保证相邻的节点颜色不同，那么这个图就是二分图。

## 1. 判断是否为二分图

785\. Is Graph Bipartite? (Medium)

[Leetcode](https://leetcode.com/problems/is-graph-bipartite/description/) / [力扣](https://leetcode-cn.com/problems/is-graph-bipartite/description/)

```html
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation:
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.
```

```html
Example 2:
Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation:
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
```

```c++
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int graph_length = graph.size();
        vector<int> color(graph_length);
        for (int i = 0; i < graph_length; ++i) {
            if (!color[i] && !coloring(graph, color, i, 1))
                return false;
        }
        return true;
    }

private:
    //DFS
    bool coloring(vector<vector<int>>& graph, vector<int>& color, int idx, int color_to) {
        if (color[idx])
            return color[idx] == color_to;
        color[idx] = color_to;
        for (const int neighbor_idx : graph[idx]) {
            if (!coloring(graph, color, neighbor_idx, -color_to))
                return false;
        }
        return true;
    }    
};
```

# 拓扑排序

常用于在具有先序关系的任务规划中。

## 1. 课程安排的合法性

207\. Course Schedule (Medium)

[Leetcode](https://leetcode.com/problems/course-schedule/description/) / [力扣](https://leetcode-cn.com/problems/course-schedule/description/)

```html
2, [[1,0]]
return true
```

```html
2, [[1,0],[0,1]]
return false
```

题目描述：一个课程可能会先修课程，判断给定的先修课程规定是否合法。

本题不需要使用拓扑排序，只需要检测有向图是否存在环即可。

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        
        for (const auto& pair : prerequisites) {
            graph[pair[1]].push_back(pair[0]);
        }   
        // not_visit = 0, visiting = 1, visited = 2.
        vector<int> status(numCourses);
        for (int course = 0; course < numCourses; ++course) {
            if (!DFS(graph, course, status)) return false;
        }
        return true;
    }
private:
    bool DFS(vector<vector<int>>& graph, int idx, vector<int>& status) {
        if (status[idx] == 2) return true;
        if (status[idx] == 1) return false;
        status[idx] = 1;
        for (const auto& neighbor : graph[idx]) {
            if (!DFS(graph, neighbor, status))
                return false;
        }
        status[idx] = 2;
        return true;
    }
};
```

## 2. 课程安排的顺序

210\. Course Schedule II (Medium)

[Leetcode](https://leetcode.com/problems/course-schedule-ii/description/) / [力扣](https://leetcode-cn.com/problems/course-schedule-ii/description/)

```html
4, [[1,0],[2,0],[3,1],[3,2]]
There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is[0,2,1,3].
```

使用 DFS 来实现拓扑排序，https://zxi.mytechroad.com/blog/graph/leetcode-210-course-schedule-ii/

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        graph_ = vector<vector<int>>(numCourses);
        // status visited == 1, visiting == 2
        status_ = vector<int>(numCourses);
        vector<int> ans;
        for (const auto& course : prerequisites)
            graph_[course[1]].push_back(course[0]);
        for (int i = 0; i < numCourses; ++i) {
            if (!DFS(graph_, status_, i, ans))
                return {};
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }

private:
    bool DFS(vector<vector<int>>& graph, vector<int>& status, int curr_course, vector<int>& ans) {
        if (status[curr_course] == 1) return true;
        if (status[curr_course] == 2) return false;
        status[curr_course] = 2;
        
        for (const auto& next_course : graph[curr_course]) {
            if (!DFS(graph, status, next_course, ans)) return false;
        }
        // post-order
        ans.push_back(curr_course);
        
        status[curr_course] = 1;
        return true;
    }
    vector<vector<int>> graph_;
    vector<int> status_;
};
```

# 并查集

并查集可以动态地连通两个点，并且可以非常快速地判断两个点是否连通。

## 1. 冗余连接

684\. Redundant Connection (Medium)

[Leetcode](https://leetcode.com/problems/redundant-connection/description/) / [力扣](https://leetcode-cn.com/problems/redundant-connection/description/)

```html
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```

题目描述：有一系列的边连成的图，找出一条边，移除它之后该图能够成为一棵树。
https://zxi.mytechroad.com/blog/tree/leetcode-684-redundant-connection/

```c++
class FindUnion {
public:
    FindUnion (int size) : parent_(size + 1, 0), size_(size + 1, 1) {
    // size + 1 because I want the list 1-based, so the index matches.
        for (int i = 0; i <= size; ++i) {
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
        return true;
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
};

class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        FindUnion fu(edges.size());
        for (const auto& pair : edges) {
            if (!fu.Union(pair[0], pair[1]))
                return pair;
        }
        return {};
    }
};
```

## 547. Friend Circles
Medium
```html
There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a N*N matrix M representing the friend relationship between students in the class. If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

Example 1:

Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
 

Example 2:

Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```
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
        return Method2(M); //Union Find
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
};
```

## 305. Number of Islands II
hard
```html
A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example:

Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
Explanation:

Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).

0 0 0
0 0 0
0 0 0
Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.

1 0 0
0 0 0   Number of islands = 1
0 0 0
Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.

1 1 0
0 0 0   Number of islands = 1
0 0 0
Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.

1 1 0
0 0 1   Number of islands = 2
0 0 0
Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.

1 1 0
0 0 1   Number of islands = 3
0 1 0
```

```c++
// A hash function used to hash a pair of any kind 
struct hash_pair { 
    template <class T1, class T2> 
    size_t operator()(const pair<T1, T2>& p) const
    { 
        auto hash1 = hash<T1>{}(p.first); 
        auto hash2 = hash<T2>{}(p.second); 
        return hash1 ^ hash2; 
    } 
};

class FindUnion {
public:
    FindUnion (int m, int n) : parent_{{pair{-1,-1}, pair{-1,-1}}}, size_{{pair{-1,-1}, 0}} {
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                pair p = make_pair(i, j);
                parent_[p] = p;
                size_[p] = 1;
            }
        }
    }
        
    bool Union(pair<int, int> a, pair<int, int> b) {
        pair<int, int> root_a = Find(a);
        pair<int, int> root_b = Find(b);
        if (root_a == root_b) return false;
        if (size_[root_a] > size_[root_b]) {
            parent_[root_b] = root_a;
            size_[root_a] += size_[root_b];
        } else {
            parent_[root_a] = root_b;
            size_[root_b] += size_[root_a];
        }
        return true;
    }
    
      
private:
    pair<int, int> Find(pair<int, int> p) {
        if (p != parent_[p]) {
            parent_[p] = Find(parent_[p]);
        }
        return parent_[p];
    }
    
    unordered_map<pair<int, int>, pair<int, int>, hash_pair> parent_;
    unordered_map<pair<int, int>, int, hash_pair> size_;  
};

class Solution {
public:
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        vector<int> ans;
        int unions = 0;
        vector<vector<int>> map(m, vector<int>(n));
        FindUnion fu(m, n);
        for (const auto& item : positions) {
            int i = item[0], j = item[1];
            if (map[i][j] == 1) {
                ans.push_back(unions);
                continue;
            }                
            map[i][j] = 1;
            unions += 1;
            pair<int, int> position_a = make_pair(i, j);
            pair<int, int> position_b(-1, -1);
            // top
            if (i - 1 >= 0 && map[i - 1][j] == 1) {
                position_b = make_pair(i - 1, j);
                if(fu.Union(position_a, position_b))
                    unions--;
            }
            // down
            if (i + 1 < m && map[i + 1][j] == 1) {
                position_b = make_pair(i + 1, j);
                if(fu.Union(position_a, position_b))
                    unions--;
            }
            // left
            if (j - 1 >= 0 && map[i][j - 1] == 1) {
                position_b = make_pair(i, j - 1);
                if(fu.Union(position_a, position_b))
                    unions--;
            }
            // right
            if (j + 1 < n && map[i][j + 1] == 1) {
                position_b = make_pair(i, j + 1);
                if(fu.Union(position_a, position_b))
                    unions--;
            }
            ans.push_back(unions);
        }
        return ans;
    }
};




```



<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
