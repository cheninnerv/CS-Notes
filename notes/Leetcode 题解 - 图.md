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

```c++
class FindUnion {
public:
    FindUnion (int size) : parent_(size + 1, 0), size_(size + 1, 1) {
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






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
