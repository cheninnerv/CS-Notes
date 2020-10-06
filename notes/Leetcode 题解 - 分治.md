<!-- GFM-TOC -->
* [1. 给表达式加括号](#1-给表达式加括号)
* [2. 不同的二叉搜索树](#2-不同的二叉搜索树)
<!-- GFM-TOC -->


# 1. 给表达式加括号

241\. Different Ways to Add Parentheses (Medium)

[Leetcode](https://leetcode.com/problems/different-ways-to-add-parentheses/description/) / [力扣](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/description/)

```html
Input: "2-1-1".

((2-1)-1) = 0
(2-(1-1)) = 2

Output : [0, 2]
```
https://zxi.mytechroad.com/blog/leetcode/leetcode-241-different-ways-to-add-parentheses/
```c++
namespace myMath {
    int add(int a, int b) {return a + b;}
    int minus(int a, int b) {return a - b;}
    int multiple(int a, int b) {return a * b;}
}
class Solution {
public:
    vector<int> diffWaysToCompute(string input) {
        if (map_.count(input)) return map_[input]; 
        bool has_operator = false;
        vector<int> ans;
        for (int i = 0; i < input.length(); ++i) {
            const char& c = input.at(i);
            if (c == '+' || c == '-' || c== '*') {
                has_operator = true;
                const auto& l = diffWaysToCompute(input.substr(0, i));
                const auto& r = diffWaysToCompute(input.substr(i + 1));
                
                std::function<int(int, int)> f;
                switch (c) {
                    case '+' : f = myMath::add; break;
                    case '-' : f = myMath::minus; break;
                    case '*' : f = myMath::multiple; break;
                }
                
                for (const int& l_item : l)
                    for (const int& r_item : r)
                        ans.push_back(f(l_item, r_item));
                    
            }
        }
        if (!input.empty() && !has_operator)
            ans.push_back(stoi(input));
        map_[input].swap(ans);
        return map_[input]; // must return map_[input], because ans is swaped to []?
    }
private:
    unordered_map<string, vector<int>> map_;
};
```

# 2. 不同的二叉搜索树

95\. Unique Binary Search Trees II (Medium)

[Leetcode](https://leetcode.com/problems/unique-binary-search-trees-ii/description/) / [力扣](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/description/)

给定一个数字 n，要求生成所有值为 1...n 的二叉搜索树。

```html
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        // 1-based
        vector<vector<vector<TreeNode*>>> cache(n + 1, vector<vector<TreeNode*>>(n + 1));
        return DFS(1, n, cache);
    }

private:
    vector<TreeNode*> DFS(int l, int r, vector<vector<vector<TreeNode*>>>& cache) {
        if (l > r) return {nullptr};
        if (!cache[l][r].empty()) return cache[l][r];
        vector<TreeNode*> ans;
        for (int i = l; i <= r; ++i)
            for (const auto& left : DFS(l, i - 1, cache))
                for (const auto& right : DFS(i + 1, r, cache)) {
                    TreeNode* node = new TreeNode(i, left, right);
                    ans.push_back(node);    
                }        
        return cache[l][r] = ans;
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
