<!-- GFM-TOC -->
* [递归](#递归)
    * [1. 树的高度](#1-树的高度)
    * [2. 平衡树](#2-平衡树)
    * [3. 两节点的最长路径](#3-两节点的最长路径)
    * [4. 翻转树](#4-翻转树)
    * [5. 归并两棵树](#5-归并两棵树)
    * [6. 判断路径和是否等于一个数](#6-判断路径和是否等于一个数)
    * [7. 统计路径和等于一个数的路径数量](#7-统计路径和等于一个数的路径数量)
    * [8. 子树](#8-子树)
    * [9. 树的对称](#9-树的对称)
    * [10. 最小路径](#10-最小路径)
    * [11. 统计左叶子节点的和](#11-统计左叶子节点的和)
    * [12. 相同节点值的最大路径长度](#12-相同节点值的最大路径长度)
    * [13. 间隔遍历](#13-间隔遍历)
    * [14. 找出二叉树中第二小的节点](#14-找出二叉树中第二小的节点)
* [层次遍历](#层次遍历)
    * [1. 一棵树每层节点的平均数](#1-一棵树每层节点的平均数)
    * [2. 得到左下角的节点](#2-得到左下角的节点)
* [前中后序遍历](#前中后序遍历)
    * [1. 非递归实现二叉树的前序遍历](#1-非递归实现二叉树的前序遍历)
    * [2. 非递归实现二叉树的后序遍历](#2-非递归实现二叉树的后序遍历)
    * [3. 非递归实现二叉树的中序遍历](#3-非递归实现二叉树的中序遍历)
* [BST](#bst)
    * [1. 修剪二叉查找树](#1-修剪二叉查找树)
    * [2. 寻找二叉查找树的第 k 个元素](#2-寻找二叉查找树的第-k-个元素)
    * [3. 把二叉查找树每个节点的值都加上比它大的节点的值](#3-把二叉查找树每个节点的值都加上比它大的节点的值)
    * [4. 二叉查找树的最近公共祖先](#4-二叉查找树的最近公共祖先)
    * [5. 二叉树的最近公共祖先](#5-二叉树的最近公共祖先)
    * [6. 从有序数组中构造二叉查找树](#6-从有序数组中构造二叉查找树)
    * [7. 根据有序链表构造平衡的二叉查找树](#7-根据有序链表构造平衡的二叉查找树)
    * [8. 在二叉查找树中寻找两个节点，使它们的和为一个给定值](#8-在二叉查找树中寻找两个节点，使它们的和为一个给定值)
    * [9. 在二叉查找树中查找两个节点之差的最小绝对值](#9-在二叉查找树中查找两个节点之差的最小绝对值)
    * [10. 寻找二叉查找树中出现次数最多的值](#10-寻找二叉查找树中出现次数最多的值)
* [Trie](#trie)
    * [1. 实现一个 Trie](#1-实现一个-trie)
    * [2. 实现一个 Trie，用来求前缀和](#2-实现一个-trie，用来求前缀和)
<!-- GFM-TOC -->


# 递归

一棵树要么是空树，要么有两个指针，每个指针指向一棵树。树是一种递归结构，很多树的问题可以使用递归来处理。

## 1. 树的高度

104\. Maximum Depth of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/description/)
求二叉树的最大深度问题用到深度优先搜索 Depth First Search，递归的完美应用，跟求二叉树的最小深度问题原理相同，参见代码如下：

```c++
public class Solution {
    public int maxDepth(TreeNode root) {
        return root == null ? 0 : (1 + Math.max(maxDepth(root.left), maxDepth(root.right)));
    }
}
```

## 2. 平衡树

110\. Balanced Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/balanced-binary-tree/description/)

```html
    3
   / \
  9  20
    /  \
   15   7
```

平衡树左右子树高度差都小于等于 1
上面那个方法正确但不是很高效，因为每一个点都会被上面的点计算深度时访问一次，我们可以进行优化。方法是如果我们发现子树不平衡，则不计算具体的深度，而是直接返回-1。那么优化后的方法为：对于每一个节点，我们通过checkDepth方法递归获得左右子树的深度，如果子树是平衡的，则返回真实的深度，若不平衡，直接返回-1，此方法时间复杂度O(N)，空间复杂度O(H)，参见代码如下：
```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (isHeightDiffByOne(root) == -1)
            return false;
        else
            return true;
    }
    
    int isHeightDiffByOne(TreeNode* root) {
        if (!root)
            return 0;
        int left = isHeightDiffByOne(root->left);
        int right = isHeightDiffByOne(root->right);
        if (left == -1 || right == -1 || abs(left - right) > 1)
            return -1;
        return max(left, right) + 1;
    }
    
};
```

## 3. 两节点的最长路径

543\. Diameter of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/diameter-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/diameter-of-binary-tree/description/)

```html
Input:

         1
        / \
       2  3
      / \
     4   5

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```

```c++
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        int res = 0;
        longestLength(root, res);
        return res;
    }
    
    int longestLength(TreeNode* root, int& max_val) {
        if (!root)
            return 0;
        if (length.count(root))
            return length[root];
        int left = longestLength(root->left, max_val);
        int right = longestLength(root->right, max_val);
        max_val = max(max_val, left + right); // not left+right+1 is because of the question definition
        length[root] = max(left, right) + 1;
        return length[root];
    }

private:
    unordered_map<TreeNode*, int> length; 
};
```

## 4. 翻转树

226\. Invert Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/invert-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/invert-binary-tree/description/)
先来看递归的方法，写法非常简洁，五行代码搞定，交换当前左右节点，并直接调用递归即可，代码如下：
```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root)
            return NULL;
        TreeNode* tmp = root->left;
        root->left = invertTree(root->right);
        root->right = invertTree(tmp);
        return root;
    }
};
```

## 5. 归并两棵树

617\. Merge Two Binary Trees (Easy)

[Leetcode](https://leetcode.com/problems/merge-two-binary-trees/description/) / [力扣](https://leetcode-cn.com/problems/merge-two-binary-trees/description/)

```html
Input:
       Tree 1                     Tree 2
          1                         2
         / \                       / \
        3   2                     1   3
       /                           \   \
      5                             4   7

Output:
         3
        / \
       4   5
      / \   \
     5   4   7
```

```c++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if (!t1) // return t2 which covers 2 cases: 1. !t1 && t2, 2. !t1 && !t2 
            return t2;
        if (!t2) // returns t1, t1 && !t2
            return t1;
        TreeNode* t = new TreeNode(t1->val + t2->val); // t1 && t2
        t->left = mergeTrees(t1->left, t2->left);
        t->right = mergeTrees(t1->right, t2->right);
        return t;
    }
};
```

## 6. 判断路径和是否等于一个数

Leetcdoe : 112. Path Sum (Easy)

[Leetcode](https://leetcode.com/problems/path-sum/description/) / [力扣](https://leetcode-cn.com/problems/path-sum/description/)

```html
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```

路径和定义为从 root 到 leaf 的所有节点的和。

```c++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (!root)
            return false;
        if (!root->left && !root->right && root->val == sum)
            return true;
        return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
    }
};
```

## 7. 统计路径和等于一个数的路径数量

437\. Path Sum III (Easy)

[Leetcode](https://leetcode.com/problems/path-sum-iii/description/) / [力扣](https://leetcode-cn.com/problems/path-sum-iii/description/)

```html
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

路径不一定以 root 开头，也不一定以 leaf 结尾，但是必须连续。

https://github.com/grandyang/leetcode/issues/437 解法2， 这题一开始hashmap怎么使用理解挺费劲的，
明确几个事情就好理解了：1，curSum一定是从root加到当前节点（包含当前节点）的值。2，root到当前节点的路径一定唯一。
3， 一定只能算当前路径（root到当前节点）上有几个解（通过hashmap查到），别的路径上的解需要递归遍历的过程中用完后立即从hashmap里抹掉，
防止重复count。4，curSum - sum = 某差值，map[某差值]的值（个数）就代表了当前路径上有几个和是sum的解。
5， map[0] = 1是程序能对所有case正常运行需要一开始设置的，在map[0] ++ --的过程中一直maintain是1，代表curSum==sum时，surSum-sum=0，map[0] =1代表有一个解。

```c++
class Solution {
public:
    int pathSum(TreeNode* root, int sum) {
        unordered_map<int, int> paths = {{0, 1}}; // accumulated_sum -> num of paths (from root to cur_node, it must be the same path with different lengths) that can lead to this accumulated_sum
        return pathSumResult(root, 0, sum, paths);
    }
    
    int pathSumResult(TreeNode* root, int curSum, int& sum, unordered_map<int, int>& paths) {
        if (!root)
            return 0;
        curSum += root->val;
        int res = paths[curSum - sum];
        ++paths[curSum];
        res += pathSumResult(root->left, curSum, sum, paths) + pathSumResult(root->right, curSum, sum, paths);
        --paths[curSum];
        return res;
    }
};
```

## 8. 子树

572\. Subtree of Another Tree (Easy)

[Leetcode](https://leetcode.com/problems/subtree-of-another-tree/description/) / [力扣](https://leetcode-cn.com/problems/subtree-of-another-tree/description/)

```html
Given tree s:
     3
    / \
   4   5
  / \
 1   2

Given tree t:
   4
  / \
 1   2

Return true, because t has the same structure and node values with a subtree of s.

Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0

Given tree t:
   4
  / \
 1   2

Return false.
```
下面这道题的解法用到了之前那道Serialize and Deserialize Binary Tree的解法，思路是对s和t两棵树分别进行序列化，各生成一个字符串，如果t的字符串是s的子串的话，就说明t是s的子树，但是需要注意的是，为了避免出现[12], [2], 这种情况，虽然2也是12的子串，但是[2]却不是[12]的子树，所以我们再序列化的时候要特殊处理一下，就是在每个结点值前面都加上一个字符，比如','，来分隔开，那么[12]序列化后就是",12,#"，而[2]序列化之后就是",2,#"，这样就可以完美的解决之前的问题了，参见代码如下：
```c++
class Solution {
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if (!s || !t) 
            return false;
        ostringstream oss_s, oss_t;
        serialize(s, oss_s);
        serialize(t, oss_t);
        if (oss_s.str().find(oss_t.str()) != string::npos)
            return true;
        return false;
    }
    
    void serialize (TreeNode* root, ostringstream& oss) {
        if (!root) {
            oss << ",#";
            return;
        }
        oss << "," << root->val;
        serialize(root->left, oss);
        serialize(root->right, oss);
    }
};
```

## 9. 树的对称

101\. Symmetric Tree (Easy)

[Leetcode](https://leetcode.com/problems/symmetric-tree/description/) / [力扣](https://leetcode-cn.com/problems/symmetric-tree/description/)

```html
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
https://github.com/grandyang/leetcode/issues/101
```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root)
            return true;
        return isSymmetric(root->left, root->right);
    }
    
    bool isSymmetric(TreeNode* left, TreeNode* right) {
        if (!left && !right)
            return true;
        if ((!left && right) || (left && !right) || (left->val != right->val))
            return false;
        return isSymmetric(left->left, right->right) && isSymmetric(left->right, right->left);
    }
};
```

## 10. 最小路径

111\. Minimum Depth of Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/description/)

二叉树的经典问题之最小深度问题就是就最短路径的节点个数，还是用深度优先搜索 DFS 来完成，万能的递归啊。首先判空，若当前结点不存在，直接返回0。然后看若左子结点不存在，那么对右子结点调用递归函数，并加1返回。反之，若右子结点不存在，那么对左子结点调用递归函数，并加1返回。若左右子结点都存在，则分别对左右子结点调用递归函数，将二者中的较小值加1返回即可，参见代码如下：

```c++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (!root) 
            return 0;
        if (!root->left)
            return 1 + minDepth(root->right);
        if (!root->right)
            return 1 + minDepth(root->left);
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

## 11. 统计左叶子节点的和

404\. Sum of Left Leaves (Easy)

[Leetcode](https://leetcode.com/problems/sum-of-left-leaves/description/) / [力扣](https://leetcode-cn.com/problems/sum-of-left-leaves/description/)

```html
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

```c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root)
            return 0;
        // add the value if this is a left leaf
        if (root->left && !root->left->left && !root->left->right)
            return root->left->val + sumOfLeftLeaves(root->right);
        return sumOfLeftLeaves(root->left) + sumOfLeftLeaves(root->right);
    }
};
```

## 12. 相同节点值的最大路径长度

687\. Longest Univalue Path (Easy)

[Leetcode](https://leetcode.com/problems/longest-univalue-path/) / [力扣](https://leetcode-cn.com/problems/longest-univalue-path/)

```html
             1
            / \
           4   5
          / \   \
         4   4   5

Output : 2
```

```c++
class Solution {
public:
    int longestUnivaluePath(TreeNode* root) {
        int ret = 0;
        longerPath(root, ret);
        return ret;
    }
    
    int longerPath(TreeNode* root, int& local_max) {
        if (!root)
            return 0;
        int left = longerPath(root->left, local_max);
        int right = longerPath(root->right, local_max);
        left = (root->left && root->left->val == root->val) ? ++left : 0;
        right = (root->right && root->right->val == root->val) ? ++right : 0;
        local_max = max(local_max, left + right);
        return max(left, right);
    }
};
```

## 13. 间隔遍历

337\. House Robber III (Medium)

[Leetcode](https://leetcode.com/problems/house-robber-iii/description/) / [力扣](https://leetcode-cn.com/problems/house-robber-iii/description/)

```html
     3
    / \
   2   3
    \   \
     3   1
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

```c++
class Solution {
public:
    int rob(TreeNode* root) {
        if (root == nullptr) return 0;
        if (val_cache_.count(root)) return val_cache_[root];
        int val = root->val;
        int l = rob(root->left);
        int r = rob(root->right);
        int ll = root->left ? rob(root->left->left) : 0;
        int lr = root->left ? rob(root->left->right) : 0;
        int rl = root->right ? rob(root->right->left) : 0;
        int rr = root->right ? rob(root->right->right) : 0;
        return val_cache_[root] = max(val + ll + lr + rl + rr, l + r);
        
    }
private:
    unordered_map<TreeNode*, int> val_cache_;
};
```

## 14. 找出二叉树中第二小的节点

671\. Second Minimum Node In a Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/description/)

```html
Input:
   2
  / \
 2   5
    / \
    5  7

Output: 5
```

一个节点要么具有 0 个或 2 个子节点，如果有子节点，那么根节点是最小的节点。

```c++
class Solution {
public:
    int findSecondMinimumValue(TreeNode* root) {
        if (!root) return -1;
        int the_smallest_val = root->val;
        return DFS(root, the_smallest_val);
    }

private:
    int DFS(TreeNode* root, int the_smallest_val) {
        if (!root) return -1;
        // find the second small value
        if (root->val > the_smallest_val) return root->val;
        // jump into the children nodes when the current val equals the_smallest_val
        int l = DFS(root->left, the_smallest_val);
        int r = DFS(root->right, the_smallest_val);
        if (l == -1) return r;
        if (r == -1) return l;
        return min(l, r);
    }
};
```

# 层次遍历

使用 BFS 进行层次遍历。不需要使用两个队列来分别存储当前层的节点和下一层的节点，因为在开始遍历一层的节点时，当前队列中的节点数就是当前层的节点数，只要控制遍历这么多节点数，就能保证这次遍历的都是当前层的节点。

## 1. 一棵树每层节点的平均数

637\. Average of Levels in Binary Tree (Easy)

[Leetcode](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/description/)

```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        if (!root) return {};
        vector<double> ans;
        queue<TreeNode*> queue {{root}};
        while (!queue.empty()) {
            int num_of_nodes = queue.size();
            double sum = 0;
            for (size_t i = 0; i < num_of_nodes; ++i) {
                TreeNode* node = queue.front();
                sum += node->val;
                if (node->left) queue.push(node->left);
                if (node->right) queue.push(node->right);          
                queue.pop();
            }
            ans.push_back(sum/num_of_nodes);
        }
        return ans;
    }
};
```

## 2. 得到左下角的节点

513\. Find Bottom Left Tree Value (Easy)

[Leetcode](https://leetcode.com/problems/find-bottom-left-tree-value/description/) / [力扣](https://leetcode-cn.com/problems/find-bottom-left-tree-value/description/)

```html
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```
只要是DFS，那么每一行最左边的结点肯定最先遍历到。
```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        int ans;
        int max_level = -1; //assume tree's level is 0 based
        Inorder(root, 0, max_level, ans);
        return ans;
    }
private:
    void Inorder(TreeNode* root, int lvl, int& max_lvl, int& ans) {
        if (!root) return;
        Inorder(root->left, lvl + 1, max_lvl, ans);
        if (lvl > max_lvl) {
            ans = root->val;
            max_lvl = lvl;
        }
        Inorder(root->right, lvl + 1, max_lvl, ans);
    }
};
```

# 前中后序遍历

```html
    1
   / \
  2   3
 / \   \
4   5   6
```

- 层次遍历顺序：[1 2 3 4 5 6]
- 前序遍历顺序：[1 2 4 5 3 6]
- 中序遍历顺序：[4 2 5 1 3 6]
- 后序遍历顺序：[4 5 2 6 3 1]

层次遍历使用 BFS 实现，利用的就是 BFS 一层一层遍历的特性；而前序、中序、后序遍历利用了 DFS 实现。

前序、中序、后序遍只是在对节点访问的顺序有一点不同，其它都相同。

① 前序

```java
void dfs(TreeNode root) {
    visit(root);
    dfs(root.left);
    dfs(root.right);
}
```

② 中序

```java
void dfs(TreeNode root) {
    dfs(root.left);
    visit(root);
    dfs(root.right);
}
```

③ 后序

```java
void dfs(TreeNode root) {
    dfs(root.left);
    dfs(root.right);
    visit(root);
}
```

## 1. 非递归实现二叉树的前序遍历

144\. Binary Tree Preorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-preorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/description/)

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root) return {};
        stack<TreeNode*> stack {{root}};
        vector<int> ans;
        while (!stack.empty()) {
            TreeNode* node = stack.top();
            stack.pop();
            ans.push_back(node->val);
            if (node->right) stack.push(node->right);
            if (node->left) stack.push(node->left);
        }
        return ans;
    }
};
```

## 2. 非递归实现二叉树的后序遍历

145\. Binary Tree Postorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/description/)

前序遍历为 root -> left -> right，后序遍历为 left -> right -> root。可以修改前序遍历成为 root -> right -> left，那么这个顺序就和后序遍历正好相反。

```c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (!root) return {};
        stack<TreeNode*> s; s.push(root);
        while (!s.empty()) {
            TreeNode* node = s.top(); 
            ans.insert(ans.begin(), node->val);
            s.pop();
            if (node->left) s.push(node->left);
            if (node->right)s.push(node->right);
        }
        return ans;
    }
};
```

## 3. 非递归实现二叉树的中序遍历

94\. Binary Tree Inorder Traversal (Medium)

[Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/description/) / [力扣](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/description/)

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
    vector<int> inorderTraversal(TreeNode* root) {
        if (!root) return {};
        vector<int> ans;
        stack<TreeNode*> s;
        TreeNode* q = root;
        while (q || !s.empty()) {
            while (q) {
                s.push(q);
                q = q->left;
            }
            TreeNode* node = s.top(); s.pop();
            ans.push_back(node->val);
            q = node->right;
        }
        return ans;
    }
};
```

# BST

二叉查找树（BST）：根节点大于等于左子树所有节点，小于等于右子树所有节点。

二叉查找树中序遍历有序。

## 1. 修剪二叉查找树

669\. Trim a Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/trim-a-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/trim-a-binary-search-tree/description/)

```html
Input:

    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output:

      3
     /
   2
  /
 1
```

题目描述：只保留值在 L \~ R 之间的节点

```c++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) return nullptr;
        if (root->val < low) return trimBST(root->right, low, high);
        if (root->val > high) return trimBST(root->left, low, high);
        // root within [low, high]
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};
```

## 2. 寻找二叉查找树的第 k 个元素

230\. Kth Smallest Element in a BST (Medium)

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/description/)


中序遍历解法：

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
    int kthSmallest(TreeNode* root, int k) {
        if (!root) return -1;
        CountedTreeNode* new_root = BuildCountedTree(root);
        return KthSmallestInCountedTree(new_root, k);
    }

private:
    struct CountedTreeNode {
        int val;
        int count;
        CountedTreeNode *left;
        CountedTreeNode *right;
        CountedTreeNode(int x) : val(x), count(0), left(nullptr), right(nullptr) {}
    };
    
    CountedTreeNode* BuildCountedTree(TreeNode* source) {
        if (!source) return nullptr;
        CountedTreeNode* node = new CountedTreeNode(source->val);
        node->left = BuildCountedTree(source->left);
        node->right = BuildCountedTree(source->right);
        int count_left = node->left ? node->left->count : 0;
        int count_right = node->right ? node->right->count : 0;
        node->count = count_left + count_right + 1;
        return node;
    }
    

    int KthSmallestInCountedTree(CountedTreeNode* root, int k) {
        if (!root) return -1;
        if (root->left) {
            if (k > root->left->count) {
                if (k == root->left->count + 1) return root->val;
                return KthSmallestInCountedTree(root->right, k - root->left->count - 1);
            }
            else {
                return KthSmallestInCountedTree(root->left, k);
            }
        } else {
            if (k == 1) return root->val; 
            return KthSmallestInCountedTree(root->right, k - 1);
        }
    }
};
```


## 3. 把二叉查找树每个节点的值都加上比它大的节点的值

538. Convert BST to Greater Tree (Easy)

[Leetcode](https://leetcode.com/problems/convert-bst-to-greater-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/description/)

```html
Input: The root of a Binary Search Tree like this:

              5
            /   \
           2     13

Output: The root of a Greater Tree like this:

             18
            /   \
          20     13
```

先遍历右子树。

```c++
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        if (!root) return nullptr;
        convertBST(root->right);
        root->val += sum;
        sum = root->val;
        convertBST(root->left);
        return root;
    }
private:
    int sum = 0;
};
```

## 4. 二叉查找树的最近公共祖先

235\. Lowest Common Ancestor of a Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

```html
        _______6______
      /                \
  ___2__             ___8__
 /      \           /      \
0        4         7        9
        /  \
       3   5

For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return nullptr;
        if (root->val < min(p->val, q->val))
            return lowestCommonAncestor(root->right, p, q);
        if (root->val > max(p->val, q->val))
            return lowestCommonAncestor(root->left, p, q);
        return root;
    }
};
```

## 5. 二叉树的最近公共祖先

236\. Lowest Common Ancestor of a Binary Tree (Medium) 

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/) / [力扣](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

```html
       _______3______
      /              \
  ___5__           ___1__
 /      \         /      \
6        2       0        8
        /  \
       7    4

For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return nullptr;
        if (root->val == p->val || root->val == q->val)
            return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left && right)
            return root;
        return left ? left : right;
    }
};
```

## 6. 从有序数组中构造二叉查找树

108\. Convert Sorted Array to Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/description/)

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        function<TreeNode* (int l, int r)> build = [&](int l, int r) {
            if (l > r) return static_cast<TreeNode*>(nullptr);
            int m = l + (r - l) / 2;
            TreeNode* node = new TreeNode(nums[m]);
            node->left = build(l, m - 1);
            node->right = build(m + 1, r);
            return node;
        };
        return build(0, nums.size() - 1);
    }
};
```

## 7. 根据有序链表构造平衡的二叉查找树

109\. Convert Sorted List to Binary Search Tree (Medium)

[Leetcode](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/description/)

```html
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    TreeNode* sortedListToBST(ListNode* head) {
        function<TreeNode* (ListNode* l, ListNode* r)> build = [&](ListNode* l, ListNode* r) {
            if (l == r) return static_cast<TreeNode*>(nullptr);
            ListNode *slow = l, *fast = l;
            while (fast != r && fast->next != r) {
                slow = slow->next;
                fast = fast->next->next;
            }
            TreeNode *node = new TreeNode(slow->val);
            node->left = build(l, slow);
            node->right = build(slow->next, r);
            return node;
        };
        return build(head, nullptr);
    }
};
```

## 8. 在二叉查找树中寻找两个节点，使它们的和为一个给定值

653\. Two Sum IV - Input is a BST (Easy)

[Leetcode](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/) / [力扣](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/description/)

```html
Input:

    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```

使用中序遍历得到有序数组之后，再利用双指针对数组进行查找。

应该注意到，这一题不能用分别在左右子树两部分来处理这种思想，因为两个待求的节点可能分别在左右子树中。

```c++
class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        if (!root) return false;
        unordered_set<int> set;
        return FindTargetByHashset(root, k, set);
    }

private:
    bool FindTargetByHashset(TreeNode* root, int k, unordered_set<int>& set) {
        if (!root) return false;
        if (set.count(k - root->val)) return true;
        set.insert(root->val);
        return FindTargetByHashset(root->left, k, set) || FindTargetByHashset(root->right, k, set);
    }
};
```

## 9. 在二叉查找树中查找两个节点之差的最小绝对值

530\. Minimum Absolute Difference in BST (Easy)

[Leetcode](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/) / [力扣](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/description/)

```html
Input:

   1
    \
     3
    /
   2

Output:

1
```

利用二叉查找树的中序遍历为有序的性质，计算中序遍历中临近的两个节点之差的绝对值，取最小值。

```c++
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        int ans = INT_MAX;
        InorderCalculateMin(root, ans);
        return ans;
    }

private:
    void InorderCalculateMin(TreeNode* root, int& ans) {
        if (!root) return;
        InorderCalculateMin(root->left, ans);
        if (pre_)
            ans = min(ans, root->val - *pre_);
        pre_ = &root->val;
        InorderCalculateMin(root->right, ans);
    }
    
    int* pre_ = nullptr;
};
```

## 10. 寻找二叉查找树中出现次数最多的值

501\. Find Mode in Binary Search Tree (Easy)

[Leetcode](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/) / [力扣](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/description/)

```html
   1
    \
     2
    /
   2

return [2].
```

答案可能不止一个，也就是有多个值出现的次数一样多。

```c++
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        vector<int> ans;
        int cnt = 0, max = 0;
        pre_ = nullptr;
        InorderFindMode(root, cnt, max, ans);
        return ans;
    }

private:
    void InorderFindMode(TreeNode* root, int& cnt, int& max, vector<int>& ans) {
        if (!root) return;
        InorderFindMode(root->left, cnt, max, ans);
        if (pre_) {
            cnt = root->val == pre_->val ? cnt + 1 : 1;
        } else {
            cnt = 1;
        }
        if (cnt > max) {
            ans.clear();
            ans.push_back(root->val);
            max = cnt;
        } else if (cnt == max) {
            ans.push_back(root->val);
        }
        pre_ = root;
        InorderFindMode(root->right, cnt, max, ans);
        
    }
TreeNode* pre_;
};
```

# Trie

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/5c638d59-d4ae-4ba4-ad44-80bdc30f38dd.jpg"/> </div><br>

Trie，又称前缀树或字典树，用于判断字符串是否存在或者是否具有某种字符串前缀。

## 1. 实现一个 Trie

208\. Implement Trie (Prefix Tree) (Medium)

[Leetcode](https://leetcode.com/problems/implement-trie-prefix-tree/description/) / [力扣](https://leetcode-cn.com/problems/implement-trie-prefix-tree/description/)

```c++
class Trie {
public:
    /** Initialize your data structure here. */
    Trie():root_(new TrieNode()){}

    
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode* p = root_.get();
        for (const char c : word) {
            if (!p->children[c - 'a']) {
                p->children[c - 'a'] = new TrieNode();
            }
            p = p->children[c - 'a']; 
        }
        p->is_word = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        TrieNode* p = root_.get();
        for (const char c : word) {
            if (p->children[c - 'a']) 
                p = p->children[c - 'a'];
            else
                return false;
        }
        return p->is_word;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        TrieNode* p = root_.get();
        for (const char c : prefix) {
            if (p->children[c - 'a']) 
                p = p->children[c - 'a'];
            else
                return false;
        }
        return true;
    }

private:
    struct TrieNode {
        TrieNode() : is_word(false), children(26, nullptr){}
        ~TrieNode() {
            for (TrieNode* child : children) {
                if (child) delete child;
            }
        }
        bool is_word;
        vector<TrieNode*> children;
    };
    
    bool Find(TrieNode* p, const string& word) {
        if (!p) return false;
        for (const char c : word) {
            if (p->children[c - 'a']) 
                p = p->children[c - 'a'];
            else
                return false;
        }
        return true;
    }
    
    std::unique_ptr<TrieNode> root_;
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## 2. 实现一个 Trie，用来求前缀和

677\. Map Sum Pairs (Medium)

[Leetcode](https://leetcode.com/problems/map-sum-pairs/description/) / [力扣](https://leetcode-cn.com/problems/map-sum-pairs/description/)

```html
Input: insert("apple", 3), Output: Null
Input: sum("ap"), Output: 3
Input: insert("app", 2), Output: Null
Input: sum("ap"), Output: 5
```

```java
class MapSum {

    private class Node {
        Node[] child = new Node[26];
        int value;
    }

    private Node root = new Node();

    public MapSum() {

    }

    public void insert(String key, int val) {
        insert(key, root, val);
    }

    private void insert(String key, Node node, int val) {
        if (node == null) return;
        if (key.length() == 0) {
            node.value = val;
            return;
        }
        int index = indexForChar(key.charAt(0));
        if (node.child[index] == null) {
            node.child[index] = new Node();
        }
        insert(key.substring(1), node.child[index], val);
    }

    public int sum(String prefix) {
        return sum(prefix, root);
    }

    private int sum(String prefix, Node node) {
        if (node == null) return 0;
        if (prefix.length() != 0) {
            int index = indexForChar(prefix.charAt(0));
            return sum(prefix.substring(1), node.child[index]);
        }
        int sum = node.value;
        for (Node child : node.child) {
            sum += sum(prefix, child);
        }
        return sum;
    }

    private int indexForChar(char c) {
        return c - 'a';
    }
}
```







<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
