<!-- GFM-TOC -->
* [斐波那契数列](#斐波那契数列)
    * [1. 爬楼梯](#1-爬楼梯)
    * [2. 强盗抢劫](#2-强盗抢劫)
    * [3. 强盗在环形街区抢劫](#3-强盗在环形街区抢劫)
    * [4. 信件错排](#4-信件错排)
    * [5. 母牛生产](#5-母牛生产)
* [矩阵路径](#矩阵路径)
    * [1. 矩阵的最小路径和](#1-矩阵的最小路径和)
    * [2. 矩阵的总路径数](#2-矩阵的总路径数)
* [数组区间](#数组区间)
    * [1. 数组区间和](#1-数组区间和)
    * [2. 数组中等差递增子区间的个数](#2-数组中等差递增子区间的个数)
* [分割整数](#分割整数)
    * [1. 分割整数的最大乘积](#1-分割整数的最大乘积)
    * [2. 按平方数来分割整数](#2-按平方数来分割整数)
    * [3. 分割整数构成字母字符串](#3-分割整数构成字母字符串)
* [最长递增子序列](#最长递增子序列)
    * [1. 最长递增子序列](#1-最长递增子序列)
    * [2. 一组整数对能够构成的最长链](#2-一组整数对能够构成的最长链)
    * [3. 最长摆动子序列](#3-最长摆动子序列)
* [最长公共子序列](#最长公共子序列)
    * [1. 最长公共子序列](#1-最长公共子序列)
* [0-1 背包](#0-1-背包)
    * [1. 划分数组为和相等的两部分](#1-划分数组为和相等的两部分)
    * [2. 改变一组数的正负号使得它们的和为一给定数](#2-改变一组数的正负号使得它们的和为一给定数)
    * [3. 01 字符构成最多的字符串](#3-01-字符构成最多的字符串)
    * [4. 找零钱的最少硬币数](#4-找零钱的最少硬币数)
    * [5. 找零钱的硬币数组合](#5-找零钱的硬币数组合)
    * [6. 字符串按单词列表分割](#6-字符串按单词列表分割)
    * [7. 组合总和](#7-组合总和)
* [股票交易](#股票交易)
    * [1. 只能进行一次的股票交易](#1-只能进行一次的股票交易)
    * [2. 只能进行无数次的股票交易](#2-只能进行无数次的股票交易)
    * [3. 只能进行两次的股票交易](#3-只能进行两次的股票交易)
    * [4. 只能进行 k 次的股票交易](#4-只能进行-k-次的股票交易)
    * [5. 需要冷却期的股票交易](#5-需要冷却期的股票交易)
    * [6. 需要交易费用的股票交易](#6-需要交易费用的股票交易)
* [字符串编辑](#字符串编辑)
    * [1. 删除两个字符串的字符使它们相等](#1-删除两个字符串的字符使它们相等)
    * [2. 编辑距离](#2-编辑距离)
    * [3. 复制粘贴字符](#3-复制粘贴字符)
<!-- GFM-TOC -->




递归和动态规划都是将原问题拆成多个子问题然后求解，他们之间最本质的区别是，动态规划保存了子问题的解，避免重复计算。

# 斐波那契数列

509. Fibonacci Number
```html
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
Given N, calculate F(N).
```

```c++
class Solution {
public:
    int fib(int N) {
        if (N < 2) return N;
        int ans = 0, d0 = 0, d1 = 1;
        for (int i = 2; i <= N; ++i) {
            ans = d0 + d1;
            d0 = d1;
            d1 = ans;
        }
        return ans;
    }
};
```

## 1. 爬楼梯

70\. Climbing Stairs (Easy)

[Leetcode](https://leetcode.com/problems/climbing-stairs/description/) / [力扣](https://leetcode-cn.com/problems/climbing-stairs/description/)

题目描述：有 N 阶楼梯，每次可以上一阶或者两阶，求有多少种上楼梯的方法。

定义一个数组 dp 存储上楼梯的方法数（为了方便讨论，数组下标从 1 开始），dp[i] 表示走到第 i 个楼梯的方法数目。

第 i 个楼梯可以从第 i-1 和 i-2 个楼梯再走一步到达，走到第 i 个楼梯的方法数为走到第 i-1 和第 i-2 个楼梯的方法数之和。

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i]=dp[i-1]+dp[i-2]" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/14fe1e71-8518-458f-a220-116003061a83.png" width="200px"> </div><br>

考虑到 dp[i] 只与 dp[i - 1] 和 dp[i - 2] 有关，因此可以只用两个变量来存储 dp[i - 1] 和 dp[i - 2]，使得原来的 O(N) 空间复杂度优化为 O(1) 复杂度。

```c++
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        int ans = 0, s1 = 1, s2 = 2;
        for (int i = 3; i <= n; ++i) {
            ans = s1 + s2;
            s1 = s2;
            s2 = ans;
        }
        return ans;
    }
};
```

## 2. 强盗抢劫

198\. House Robber (Easy)

[Leetcode](https://leetcode.com/problems/house-robber/description/) / [力扣](https://leetcode-cn.com/problems/house-robber/description/)

题目描述：抢劫一排住户，但是不能抢邻近的住户，求最大抢劫量。

定义 dp 数组用来存储最大的抢劫量，其中 dp[i] 表示抢到第 i 个住户时的最大抢劫量。

由于不能抢劫邻近住户，如果抢劫了第 i -1 个住户，那么就不能再抢劫第 i 个住户，所以

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i]=max(dp[i-2]+nums[i],dp[i-1])" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/2de794ca-aa7b-48f3-a556-a0e2708cb976.jpg" width="350px"> </div><br>

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        // dp(n) = max (dp(n - 2) + nums[n], dp(n - 1));
        int dp1 = 0, dp2 = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int dp = max(dp2 + nums[i], dp1); 
            dp2 = dp1;
            dp1 = dp;
        }
        return dp1;
    }
};
```

## 3. 强盗在环形街区抢劫

213\. House Robber II (Medium)

[Leetcode](https://leetcode.com/problems/house-robber-ii/description/) / [力扣](https://leetcode-cn.com/problems/house-robber-ii/description/)

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 1) return nums[0];
        int dp1 = 0, dp2 = 0, ans = 0;
        // rob 1st, not rob last
        for (int i = 0; i < nums.size() - 1; ++i) {
            int dp = max(dp2 + nums[i], dp1);
            dp2 = dp1;
            dp1 = dp;
        }
        ans = dp1;    
        // rob last, not rob 1st
        dp1 = 0, dp2 = 0;
        for (int i = 1; i < nums.size(); ++i) {
            int dp = max(dp2 + nums[i], dp1);
            dp2 = dp1;
            dp1 = dp;
        }
        return max(ans, dp1);
    }
};
```

## 4. 信件错排 (1-2year 跳过这题）

题目描述：有 N 个 信 和 信封，它们被打乱，求错误装信方式的数量。

定义一个数组 dp 存储错误方式数量，dp[i] 表示前 i 个信和信封的错误方式数量。假设第 i 个信装到第 j 个信封里面，而第 j 个信装到第 k 个信封里面。根据 i 和 k 是否相等，有两种情况：

- i==k，交换 i 和 j 的信后，它们的信和信封在正确的位置，但是其余 i-2 封信有 dp[i-2] 种错误装信的方式。由于 j 有 i-1 种取值，因此共有 (i-1)\*dp[i-2] 种错误装信方式。
- i != k，交换 i 和 j 的信后，第 i 个信和信封在正确的位置，其余 i-1 封信有 dp[i-1] 种错误装信方式。由于 j 有 i-1 种取值，因此共有 (i-1)\*dp[i-1] 种错误装信方式。

综上所述，错误装信数量方式数量为：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i]=(i-1)*dp[i-2]+(i-1)*dp[i-1]" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/da1f96b9-fd4d-44ca-8925-fb14c5733388.png" width="350px"> </div><br>

634. Find the Derangement of An Array
```html
In combinatorial mathematics, a derangement is a permutation of the elements of a set, such that no element appears in its original position.

There's originally an array consisting of n integers from 1 to n in ascending order, you need to find the number of derangement it can generate.

Also, since the answer may be very large, you should return the output mod 109 + 7.

Example 1:
Input: 3
Output: 2
Explanation: The original array is [1,2,3]. The two derangements are [2,3,1] and [3,1,2]
```
https://leetcode.com/problems/find-the-derangement-of-an-array/submissions/
https://github.com/grandyang/leetcode/issues/634
```c++
class Solution {
public:
    int findDerangement(int n) {
        if (n < 2) return 0;
        vector<long long> dp(n + 1, 0);
        dp[1] = 0; dp[2] = 1;
        for (int i = 3; i <= n; ++i) {
            dp[i] = (dp[i - 1] + dp[i - 2]) * (i - 1) % 1000000007;
        }
        return dp[n];
    }
};
```

## 5. 母牛生产

[程序员代码面试指南-P181](#)

题目描述：假设农场中成熟的母牛每年都会生 1 头小母牛，并且永远不会死。第一年有 1 只小母牛，从第二年开始，母牛开始生小母牛。每只小母牛 3 年之后成熟又可以生小母牛。给定整数 N，求 N 年后牛的数量。

第 i 年成熟的牛的数量为：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i]=dp[i-1]+dp[i-3]" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/879814ee-48b5-4bcb-86f5-dcc400cb81ad.png" width="250px"> </div><br>

# 矩阵路径

## 1. 矩阵的最小路径和

64\. Minimum Path Sum (Medium)

[Leetcode](https://leetcode.com/problems/minimum-path-sum/description/) / [力扣](https://leetcode-cn.com/problems/minimum-path-sum/description/)

```html
[[1,3,1],
 [1,5,1],
 [4,2,1]]
Given the above grid map, return 7. Because the path 1→3→1→1→1 minimizes the sum.
```

题目描述：求从矩阵的左上角到右下角的最小路径和，每次只能向右和向下移动。

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;
        int m = grid.size(), n = grid[0].size();
        vector<int> dp(n, INT_MAX); dp[0] = 0;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j) {
                if (j == 0)
                    dp[j] += grid[i][j];
                else
                    dp[j] = min(dp[j - 1], dp[j]) + grid[i][j];
            }
        return dp[n - 1];
    }
};
```

## 2. 矩阵的总路径数

62\. Unique Paths (Medium)

[Leetcode](https://leetcode.com/problems/unique-paths/description/) / [力扣](https://leetcode-cn.com/problems/unique-paths/description/)

题目描述：统计从矩阵左上角到右下角的路径总数，每次只能向右或者向下移动。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/dc82f0f3-c1d4-4ac8-90ac-d5b32a9bd75a.jpg" width=""> </div><br>

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        if (m == 0 || n == 0) return 0;
        vector<int> dp(n, 0); dp[0] = 1;
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j) {
                if (j == 0)
                    continue;
                else
                    dp[j] = dp[j] + dp[j - 1];
            }
        return dp[n - 1];
    }
};
```

也可以直接用数学公式求解，这是一个组合问题。机器人总共移动的次数 S=m+n-2，向下移动的次数 D=m-1，那么问题可以看成从 S 中取出 D 个位置的组合数量，这个问题的解为 C(S, D)。

```java
public int uniquePaths(int m, int n) {
    int S = m + n - 2;  // 总共的移动次数
    int D = m - 1;      // 向下的移动次数
    long ret = 1;
    for (int i = 1; i <= D; i++) {
        ret = ret * (S - D + i) / i;
    }
    return (int) ret;
}
```

# 数组区间

## 1. 数组区间和

303\. Range Sum Query - Immutable (Easy)

[Leetcode](https://leetcode.com/problems/range-sum-query-immutable/description/) / [力扣](https://leetcode-cn.com/problems/range-sum-query-immutable/description/)

```html
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

求区间 i \~ j 的和，可以转换为 sum[j] - sum[i - 1]，其中 sum[i] 为 0 \~ i 的和。

```c++
class NumArray {
public:
    NumArray(vector<int>& nums) {
        if (nums.empty()) return;
        sum_ = vector<int>(nums.size());
        sum_[0] = nums[0];
        for (int i = 1; i < nums.size(); ++i)
            sum_[i] = sum_[i - 1] + nums[i];
    }
    
    int sumRange(int i, int j) {
        if (i == 0)
            return sum_[j];
        else
            return sum_[j] - sum_[i - 1];
    }
private:
    vector<int> sum_;
};
```

## 2. 数组中等差递增子区间的个数

413\. Arithmetic Slices (Medium)

[Leetcode](https://leetcode.com/problems/arithmetic-slices/description/) / [力扣](https://leetcode-cn.com/problems/arithmetic-slices/description/)

```html
A = [0, 1, 2, 3, 4]

return: 6, for 3 arithmetic slices in A:

[0, 1, 2],
[1, 2, 3],
[0, 1, 2, 3],
[0, 1, 2, 3, 4],
[ 1, 2, 3, 4],
[2, 3, 4]
```

dp[i] 表示以 A[i] 为结尾的等差递增子区间的个数。

当 A[i] - A[i-1] == A[i-1] - A[i-2]，那么 [A[i-2], A[i-1], A[i]] 构成一个等差递增子区间。而且在以 A[i-1] 为结尾的递增子区间的后面再加上一个 A[i]，一样可以构成新的递增子区间。

```html
dp[2] = 1
    [0, 1, 2]
dp[3] = dp[2] + 1 = 2
    [0, 1, 2, 3], // [0, 1, 2] 之后加一个 3
    [1, 2, 3]     // 新的递增子区间
dp[4] = dp[3] + 1 = 3
    [0, 1, 2, 3, 4], // [0, 1, 2, 3] 之后加一个 4
    [1, 2, 3, 4],    // [1, 2, 3] 之后加一个 4
    [2, 3, 4]        // 新的递增子区间
```

综上，在 A[i] - A[i-1] == A[i-1] - A[i-2] 时，dp[i] = dp[i-1] + 1。

因为递增子区间不一定以最后一个元素为结尾，可以是任意一个元素结尾，因此需要返回 dp 数组累加的结果。

```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        if (A.size() < 3) return 0;
        int ans = 0, l = 0, n = A.size(), pre = A[1] - A[0];
        for (int r = 2; r <= n; ++r) {
            if (r == n || A[r] - A[r - 1] != pre) {
                int cnt = r - 1 - l + 1;
                if (cnt >= 3)
                    ans += numberOfArithmetic(cnt);
                l = r - 1;
                if (r < n) pre = A[r] - A[r - 1];
            }
        }
        return ans;
    }
    
    int numberOfArithmetic(int n) {
        // int ans = 0, s = 3;
        // while (s <= n) {
        //     ans += n - s + 1;
        //     s++;
        // }
        // return ans;
        
        return ((n - 1) * (n - 2)) / 2;
    }
};
```

# 分割整数

## 1. 分割整数的最大乘积

343\. Integer Break (Medim)

[Leetcode](https://leetcode.com/problems/integer-break/description/) / [力扣](https://leetcode-cn.com/problems/integer-break/description/)

题目描述：For example, given n = 2, return 1 (2 = 1 + 1); given n = 10, return 36 (10 = 3 + 3 + 4).

```c++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1, 1);
        for (int i = 3; i <= n; ++i) {
            for (int j = 1; j < i; ++j) {
                dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
};
```

## 2. 按平方数来分割整数

279\. Perfect Squares(Medium)

[Leetcode](https://leetcode.com/problems/perfect-squares/description/) / [力扣](https://leetcode-cn.com/problems/perfect-squares/description/)

题目描述：For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

```c++
class Solution {
public:
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
    
    // math
    // int numSquares(int n) {
    //     while (n % 4 == 0) n /= 4;
    //     if (n % 8 == 7) return 4;
    //     for (int a = 0; a * a <= n; ++a) {
    //         int b = sqrt(n - a * a);
    //         if (a * a + b * b == n) {
    //             return a > 0 && b > 0 ? 2 : 1;
    //         }
    //     }
    //     return 3;
    // }
};
```

## 3. 分割整数构成字母字符串

91\. Decode Ways (Medium)

[Leetcode](https://leetcode.com/problems/decode-ways/description/) / [力扣](https://leetcode-cn.com/problems/decode-ways/description/)

题目描述：Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

```c++
class Solution {
public:
    int numDecodings(string s) {
        cache_ = vector<int>(s.size(), -1);
        int ans = numDecodings(s, 0);
        return ans;
    }
private:
    int numDecodings(string s, int idx) {
        if (s.empty()) return 1; 
        if (cache_[idx] != -1) return cache_[idx];
        int ans = 0;
        for (int i = 1; i <= 2; ++i) {
            if (i > s.size()) break;
            string cur = s.substr(0, i);
            if (cur[0] == '0') return 0;
            if (stoi(cur) >= 1 && stoi(cur) <= 26)
                ans += numDecodings(s.substr(i), idx + i);
        }
        return cache_[idx] = ans;
    }
    
    vector<int> cache_;
};
```

# 最长递增子序列

已知一个序列 {S<sub>1</sub>, S<sub>2</sub>,...,S<sub>n</sub>}，取出若干数组成新的序列 {S<sub>i1</sub>, S<sub>i2</sub>,..., S<sub>im</sub>}，其中 i1、i2 ... im 保持递增，即新序列中各个数仍然保持原数列中的先后顺序，称新序列为原序列的一个  **子序列**  。

如果在子序列中，当下标 ix > iy 时，S<sub>ix</sub> > S<sub>iy</sub>，称子序列为原序列的一个  **递增子序列**  。

定义一个数组 dp 存储最长递增子序列的长度，dp[n] 表示以 S<sub>n</sub> 结尾的序列的最长递增子序列长度。对于一个递增子序列 {S<sub>i1</sub>, S<sub>i2</sub>,...,S<sub>im</sub>}，如果 im < n 并且 S<sub>im</sub> < S<sub>n</sub>，此时 {S<sub>i1</sub>, S<sub>i2</sub>,..., S<sub>im</sub>, S<sub>n</sub>} 为一个递增子序列，递增子序列的长度增加 1。满足上述条件的递增子序列中，长度最长的那个递增子序列就是要找的，在长度最长的递增子序列上加上 S<sub>n</sub> 就构成了以 S<sub>n</sub> 为结尾的最长递增子序列。因此 dp[n] = max{ dp[i]+1 | S<sub>i</sub> < S<sub>n</sub> && i < n} 。

因为在求 dp[n] 时可能无法找到一个满足条件的递增子序列，此时 {S<sub>n</sub>} 就构成了递增子序列，需要对前面的求解方程做修改，令 dp[n] 最小为 1，即：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[n]=max\{1,dp[i]+1|S_i<S_n\&\&i<n\}" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ee994da4-0fc7-443d-ac56-c08caf00a204.jpg" width="350px"> </div><br>

对于一个长度为 N 的序列，最长递增子序列并不一定会以 S<sub>N</sub> 为结尾，因此 dp[N] 不是序列的最长递增子序列的长度，需要遍历 dp 数组找出最大值才是所要的结果，max{ dp[i] | 1 <= i <= N} 即为所求。

## 1. 最长递增子序列

300\. Longest Increasing Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/longest-increasing-subsequence/description/)

```c++
class Solution {
public:
    // Method 1, O(n^2) recursion + memory
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        int ans = 0;
        vector<int> cache(nums.size());
        for (int i = nums.size() - 1; i >= 0; --i)
            ans = max(ans, DFS(nums, i, cache));
        return ans;
    }
    
    int DFS(vector<int>& nums, int i, vector<int>& cache) {
        if (i == 0) return 1;
        if (cache[i] > 0) return cache[i];
        int pre = 0;
        for (int j = i - 1; j >= 0; --j) {
            if (nums[j] < nums[i])
                pre = max(pre, DFS(nums, j, cache));
        }
        return cache[i] = pre + 1;
    }
    
    // Method 2, O(nlogn) binary search (a card game: patience sort)
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        vector<int> stacks;
        for (int i = 0; i < nums.size(); ++i) {
            // lower_bound returns first >= item's idx
            auto it = lower_bound(stacks.begin(), stacks.end(), nums[i]);
            if (it == stacks.end()) stacks.push_back(nums[i]);
            else *it = nums[i];
        }
        return stacks.size();
    }
};
```


## 2. 一组整数对能够构成的最长链

646\. Maximum Length of Pair Chain (Medium)

[Leetcode](https://leetcode.com/problems/maximum-length-of-pair-chain/description/) / [力扣](https://leetcode-cn.com/problems/maximum-length-of-pair-chain/description/)

```html
Input: [[1,2], [2,3], [3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4]
```

题目描述：对于 (a, b) 和 (c, d) ，如果 b < c，则它们可以构成一条链。

```c++
class Solution {
public:
    int findLongestChain(vector<vector<int>>& pairs) {
        sort(pairs.begin(), pairs.end(), [&](vector<int>& a, vector<int>& b) {return a[1] < b[1];});
        int ans = 1, j = 0;
        for (int i = 1; i < pairs.size(); ++i) {
            if (pairs[j][1] < pairs[i][0]) {
                ans++;
                j = i;
            }
        }
        return ans;
    }
};
```

## 3. 最长摆动子序列

376\. Wiggle Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/wiggle-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/wiggle-subsequence/description/)

```html
Input: [1,7,4,9,2,5]
Output: 6
The entire sequence is a wiggle sequence.

Input: [1,17,5,10,13,15,10,5,16,8]
Output: 7
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

Input: [1,2,3,4,5,6,7,8,9]
Output: 2
```

要求：使用 O(N) 时间复杂度求解。

```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() < 2) return nums.size();
        // 1: look for positive, -1: look for negtive
        int status = 0;
        int ans = 1;
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i] == nums[i - 1]) continue;
            if (status == 0) {
                status = nums[i] > nums[i - 1] ? -1 : 1; 
                ans++; continue;
            }
            if ((nums[i] > nums[i - 1] && status == 1) ||
                (nums[i] < nums[i - 1] && status == -1)) {
                ans ++;
                status = -1 * status;
            }    
        }
        return ans;
    }
};
```

# 最长公共子序列

对于两个子序列 S1 和 S2，找出它们最长的公共子序列。

定义一个二维数组 dp 用来存储最长公共子序列的长度，其中 dp[i][j] 表示 S1 的前 i 个字符与 S2 的前 j 个字符最长公共子序列的长度。考虑 S1<sub>i</sub> 与 S2<sub>j</sub> 值是否相等，分为两种情况：

- 当 S1<sub>i</sub>==S2<sub>j</sub> 时，那么就能在 S1 的前 i-1 个字符与 S2 的前 j-1 个字符最长公共子序列的基础上再加上 S1<sub>i</sub> 这个值，最长公共子序列长度加 1，即 dp[i][j] = dp[i-1][j-1] + 1。
- 当 S1<sub>i</sub> != S2<sub>j</sub> 时，此时最长公共子序列为 S1 的前 i-1 个字符和 S2 的前 j 个字符最长公共子序列，或者 S1 的前 i 个字符和 S2 的前 j-1 个字符最长公共子序列，取它们的最大者，即 dp[i][j] = max{ dp[i-1][j], dp[i][j-1] }。

综上，最长公共子序列的状态转移方程为：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i][j]=\left\{\begin{array}{rcl}dp[i-1][j-1]&&{S1_i==S2_j}\\max(dp[i-1][j],dp[i][j-1])&&{S1_i<>S2_j}\end{array}\right." class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ecd89a22-c075-4716-8423-e0ba89230e9a.jpg" width="450px"> </div><br>

对于长度为 N 的序列 S<sub>1</sub> 和长度为 M 的序列 S<sub>2</sub>，dp[N][M] 就是序列 S<sub>1</sub> 和序列 S<sub>2</sub> 的最长公共子序列长度。

与最长递增子序列相比，最长公共子序列有以下不同点：

- 针对的是两个序列，求它们的最长公共子序列。
- 在最长递增子序列中，dp[i] 表示以 S<sub>i</sub> 为结尾的最长递增子序列长度，子序列必须包含 S<sub>i</sub> ；在最长公共子序列中，dp[i][j] 表示 S1 中前 i 个字符与 S2 中前 j 个字符的最长公共子序列长度，不一定包含 S1<sub>i</sub> 和 S2<sub>j</sub>。
- 在求最终解时，最长公共子序列中 dp[N][M] 就是最终解，而最长递增子序列中 dp[N] 不是最终解，因为以 S<sub>N</sub> 为结尾的最长递增子序列不一定是整个序列最长递增子序列，需要遍历一遍 dp 数组找到最大者。

## 1. 最长公共子序列

1143\. Longest Common Subsequence

[Leetcode](https://leetcode.com/problems/longest-common-subsequence/) / [力扣](https://leetcode-cn.com/problems/longest-common-subsequence/)

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<int> dp(text2.length() + 1, 0);
        int tmp = 0;
        for (int i = 1; i <= text1.length(); ++i)
            for (int j = 1; j <= text2.length(); ++j) {
                if (j == 1) tmp = 0;
                if (text1[i - 1] == text2[j - 1]) {
                    int n = tmp + 1;
                    tmp = dp[j];
                    dp[j] = n;
                } else {
                    tmp = dp[j];
                    dp[j] = max(dp[j], dp[j - 1]);
                }
            }
        return dp.back();        
    }
    
    // Method 2: Recursion
    int longestCommonSubsequence(string& t1, string& t2, int i, int j) {
        if (i < 0 || j < 0) return 0;
        if (t1[i] == t2[j]) return 1 + longestCommonSubsequence(t1, t2, i - 1, j - 1);
        return max(longestCommonSubsequence(t1, t2, i - 1, j),
                   longestCommonSubsequence(t1, t2, i, j - 1));
    }
};
```



# 0-1 背包

有一个容量为 N 的背包，要用这个背包装下物品的价值最大，这些物品有两个属性：体积 w 和价值 v。

定义一个二维数组 dp 存储最大价值，其中 dp[i][j] 表示前 i 件物品体积不超过 j 的情况下能达到的最大价值。设第 i 件物品体积为 w，价值为 v，根据第 i 件物品是否添加到背包中，可以分两种情况讨论：

- 第 i 件物品没添加到背包，总体积不超过 j 的前 i 件物品的最大价值就是总体积不超过 j 的前 i-1 件物品的最大价值，dp[i][j] = dp[i-1][j]。
- 第 i 件物品添加到背包中，dp[i][j] = dp[i-1][j-w] + v。

第 i 件物品可添加也可以不添加，取决于哪种情况下最大价值更大。因此，0-1 背包的状态转移方程为：

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[i][j]=max(dp[i-1][j],dp[i-1][j-w]+v)" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/8cb2be66-3d47-41ba-b55b-319fc68940d4.png" width="400px"> </div><br>

```java
// W 为背包总体积
// N 为物品数量
// weights 数组存储 N 个物品的重量
// values 数组存储 N 个物品的价值
public int knapsack(int W, int N, int[] weights, int[] values) {
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        int w = weights[i - 1], v = values[i - 1];
        for (int j = 1; j <= W; j++) {
            if (j >= w) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - w] + v);
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[N][W];
}
```

**空间优化**  

在程序实现时可以对 0-1 背包做优化。观察状态转移方程可以知道，前 i 件物品的状态仅与前 i-1 件物品的状态有关，因此可以将 dp 定义为一维数组，其中 dp[j] 既可以表示 dp[i-1][j] 也可以表示 dp[i][j]。此时，

<!--<div align="center"><img src="https://latex.codecogs.com/gif.latex?dp[j]=max(dp[j],dp[j-w]+v)" class="mathjax-pic"/></div> <br>-->

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/9ae89f16-7905-4a6f-88a2-874b4cac91f4.jpg" width="300px"> </div><br>

因为 dp[j-w] 表示 dp[i-1][j-w]，因此不能先求 dp[i][j-w]，防止将 dp[i-1][j-w] 覆盖。也就是说要先计算 dp[i][j] 再计算 dp[i][j-w]，在程序实现时需要按倒序来循环求解。

```java
public int knapsack(int W, int N, int[] weights, int[] values) {
    int[] dp = new int[W + 1];
    for (int i = 1; i <= N; i++) {
        int w = weights[i - 1], v = values[i - 1];
        for (int j = W; j >= 1; j--) {
            if (j >= w) {
                dp[j] = Math.max(dp[j], dp[j - w] + v);
            }
        }
    }
    return dp[W];
}
```

**无法使用贪心算法的解释**  

0-1 背包问题无法使用贪心算法来求解，也就是说不能按照先添加性价比最高的物品来达到最优，这是因为这种方式可能造成背包空间的浪费，从而无法达到最优。考虑下面的物品和一个容量为 5 的背包，如果先添加物品 0 再添加物品 1，那么只能存放的价值为 16，浪费了大小为 2 的空间。最优的方式是存放物品 1 和物品 2，价值为 22.

| id | w | v | v/w |
| --- | --- | --- | --- |
| 0 | 1 | 6 | 6 |
| 1 | 2 | 10 | 5 |
| 2 | 3 | 12 | 4 |

**变种**  

- 完全背包：物品数量为无限个

- 多重背包：物品数量有限制

- 多维费用背包：物品不仅有重量，还有体积，同时考虑这两种限制

- 其它：物品之间相互约束或者依赖

## 1. 划分数组为和相等的两部分

416\. Partition Equal Subset Sum (Medium)

[Leetcode](https://leetcode.com/problems/partition-equal-subset-sum/description/) / [力扣](https://leetcode-cn.com/problems/partition-equal-subset-sum/description/)

```html
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

可以看成一个背包大小为 sum/2 的 0-1 背包问题。

```c++
// Recursion + Memory
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for (const auto& n : nums)
            sum += n;
        if (sum & 1 == 1) return false;
        cache_ = vector(nums.size(), vector<int>((sum / 2) + 1, -1));
        return DFS(nums, 0, sum >> 1);
    }
private:    
    bool DFS(vector<int>& nums, int start, int sum) {
        if (sum == 0) return true;
        if (sum < 0 || start == nums.size()) return false;
        if (cache_[start][sum] != -1) return cache_[start][sum];
        for (int i = start; i < nums.size(); ++i) {
            int val = nums[i];
            if (DFS(nums, i + 1, sum - val)) return true;
        }
        cache_[start][sum] = 0;
        return false;
    }
    vector<vector<int>> cache_;
};

// DP
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        const int sum = std::accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 != 0) return false;
        vector<int> dp(sum + 1, 0);
        dp[0] = 1;
        for (const int num : nums) {
            for (int i = 0; i <= sum; ++i)
                if (dp[i] && i + num < dp.size()) dp[i + num] = 1;
            int test = sum / 2;
            if (dp[test]) return true;
        }
        return false;
    }
};
```

## 2. 改变一组数的正负号使得它们的和为一给定数

494\. Target Sum (Medium)

[Leetcode](https://leetcode.com/problems/target-sum/description/) / [力扣](https://leetcode-cn.com/problems/target-sum/description/)

```html
Input: nums is [1, 1, 1, 1, 1], S is 3.
Output: 5
Explanation:

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

该问题可以转换为 Subset Sum 问题，从而使用 0-1 背包的方法来求解。

可以将这组数看成两部分，P 和 N，其中 P 使用正号，N 使用负号，有以下推导：

```html
                  sum(P) - sum(N) = target
sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N)
                       2 * sum(P) = target + sum(nums)
```

因此只要找到一个子集，令它们都取正号，并且和等于 (target + sum(nums))/2，就证明存在解。
kc: 很经典的题，仔细阅读https://zxi.mytechroad.com/blog/dynamic-programming/leetcode-494-target-sum/
subset sum 解法：

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        const int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum < S || (sum + S) % 2 != 0) return 0;
        int target = (sum + S) / 2;
        vector<int> dp(target + 1); dp[0] = 1;
        for (const auto& item : nums) 
            for (int j = target; j >= 0; --j) {
                if (j - item >= 0) dp[j] += dp[j - item];
            }      
        return dp[target];
    }
};
```

## 3. 01 字符构成最多的字符串

474\. Ones and Zeroes (Medium)

[Leetcode](https://leetcode.com/problems/ones-and-zeroes/description/) / [力扣](https://leetcode-cn.com/problems/ones-and-zeroes/description/)

```html
Input: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
Output: 4

Explanation: There are totally 4 strings can be formed by the using of 5 0s and 3 1s, which are "10","0001","1","0"
```

这是一个多维费用的 0-1 背包问题，有两个背包大小，0 的数量和 1 的数量。

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (const auto& str : strs) {
            int zeros = 0, ones = 0;
            for (const auto& c : str) c == '0' ? zeros++ : ones++;
            for (int i = m; i >= zeros; --i)
                for (int j = n; j >= ones; --j) 
                        dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1);
        }
        return dp[m][n];        
    }
};
```

## 4. 找零钱的最少硬币数

322\. Coin Change (Medium)

[Leetcode](https://leetcode.com/problems/coin-change/description/) / [力扣](https://leetcode-cn.com/problems/coin-change/description/)

```html
Example 1:
coins = [1, 2, 5], amount = 11
return 3 (11 = 5 + 5 + 1)

Example 2:
coins = [2], amount = 3
return -1.
```

题目描述：给一些面额的硬币，要求用这些硬币来组成给定面额的钱数，并且使得硬币数量最少。硬币可以重复使用。

- 物品：硬币
- 物品大小：面额
- 物品价值：数量

因为硬币可以重复使用，因此这是一个完全背包问题。完全背包只需要将 0-1 背包的逆序遍历 dp 数组改为正序遍历即可。

```c++
class Solution {
public:
    // Time complexity: O(n * amount^2)
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, 1e5); dp[0] = 0;
        for (const auto& coin : coins) {    
            for (int i = amount - coin; i >= 0; --i) {
                if (dp[i] != 1e5) {
                    for (int k = 1; i + coin * k <= amount; ++k)
                        dp[i + coin * k] = min(dp[i] + k, dp[i + coin * k]);
                }             
            }
        }
        return dp[amount] == 1e5 ? -1 : dp[amount];
    }
    
    // Time complexity: O(n * amount)
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, 1e5); dp[0] = 0;
        for (const auto& coin : coins) {    
            for (int i = coin; i <= amount; ++i) {
                dp[i] = min(dp[i], dp[i - coin] + 1);      
            }
        }
        return dp[amount] == 1e5 ? -1 : dp[amount];
    }
};
```

## 5. 找零钱的硬币数组合

518\. Coin Change 2 (Medium)

[Leetcode](https://leetcode.com/problems/coin-change-2/description/) / [力扣](https://leetcode-cn.com/problems/coin-change-2/description/)

```text-html-basic
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1); dp[0] = 1;
        for (auto& coin : coins) {
            for (int i = coin; i <= amount; ++i) {
                dp[i] += dp[i - coin];
            }
        }
        return dp[amount];
    }
};
```

## 6. 字符串按单词列表分割

139\. Word Break (Medium)

[Leetcode](https://leetcode.com/problems/word-break/description/) / [力扣](https://leetcode-cn.com/problems/word-break/description/)

```html
s = "leetcode",
dict = ["leet", "code"].
Return true because "leetcode" can be segmented as "leet code".
```

dict 中的单词没有使用次数的限制，因此这是一个完全背包问题。

该问题涉及到字典中单词的使用顺序，也就是说物品必须按一定顺序放入背包中，例如下面的 dict 就不够组成字符串 "leetcode"：

```html
["lee", "tc", "cod"]
```

求解顺序的完全背包问题时，对物品的迭代应该放在最里层，对背包的迭代放在外层，只有这样才能让物品按一定顺序放入背包中。

```c++
    // Recursion + Memory
    bool wordBreakRecursion(string s, unordered_set<string>& wordDict) {
        if (s == "") return true;
        // memory
        if (mem_.count(s)) return mem_[s];
        if (wordDict.count(s)) return mem_[s] = true;
        
        for (size_t i = 1; i < s.length(); ++i) {
            if (wordBreakRecursion(s.substr(0, i), wordDict) && 
                wordBreakRecursion(s.substr(i, s.length() - i), wordDict)) 
                return mem_[s] = true; 
        }
        return mem_[s] = false;
    }
    
    unordered_map<string, bool> mem_;
    
    // DP
    bool wordBreakDP(string s, unordered_set<string>& wordDict) {
        vector<bool> dp(s.size() + 1); dp[0] = true;
        for (int i = 1; i <= s.size(); ++i) {
            for (int j = 0; j < i; ++j) {
                if (dp[j] && wordDict.count(s.substr(j, i - j))) {
                    dp[i] = true; 
                    break;
                }
            }
        }
        return dp.back();
    }
```

## 7. 组合总和

377\. Combination Sum IV (Medium)

[Leetcode](https://leetcode.com/problems/combination-sum-iv/description/) / [力扣](https://leetcode-cn.com/problems/combination-sum-iv/description/)

```html
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

涉及顺序的完全背包。

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned int> dp(target + 1); dp[0] = 1;
        for (int i = 1; i <= target; ++i) {
            for (auto& num : nums) {
                if (i - num >= 0) dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
};
```

# 股票交易
## 1. 只能进行一次的股票交易
```html
121. Best Time to Buy and Sell Stock
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:

Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

```c++
    // template
    int maxProfit(vector<int>& prices) {
        int dp_i10 = 0, dp_i11 = INT_MIN;
        for (const auto& p : prices) {
            dp_i10 = max(dp_i10, dp_i11 + p);
            dp_i11 = max(dp_i11, 0 - p);
        }
        return dp_i10;
    }
```

## 2. 只能进行无数次的股票交易
```html
122. Best Time to Buy and Sell Stock II
Say you have an array prices for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

Example 1:

Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```
```c++
    int maxProfit(vector<int>& prices) {
        int dp_ik0 = 0, dp_ik1 = INT_MIN;
        for (const auto& p : prices) {
            int pre_dp_ik0 = dp_ik0;
            int pre_dp_ik1 = dp_ik1;
            dp_ik0 = max(pre_dp_ik0, pre_dp_ik1 + p);
            dp_ik1 = max(pre_dp_ik1, pre_dp_ik0 - p);
        }
        return dp_ik0;
    }
```


## 3. 只能进行两次的股票交易

123\. Best Time to Buy and Sell Stock III (Hard)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/description/)

```c++
class Solution {
public:
    // template
    int maxProfit(vector<int>& prices) {
        int dp_i20 = 0, dp_i10 = 0, dp_i21 = INT_MIN, dp_i11 = INT_MIN;
        for (const auto& p : prices) {
            dp_i20 = max(dp_i20, dp_i21 + p);
            dp_i21 = max(dp_i21, dp_i10 - p);
            dp_i10 = max(dp_i10, dp_i11 + p);
            dp_i11 = max(dp_i11, 0 - p);
        }
        return dp_i20;
    }
};
```

## 4. 只能进行 k 次的股票交易

188\. Best Time to Buy and Sell Stock IV (Hard)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/description/)

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        // 这种情况下该问题退化为普通的股票可以交易无数次问题（股票题1）
        if (k > prices.size() / 2) {
            int dp_ik0 = 0, dp_ik1 = INT_MIN;
            for (const auto& p : prices) {
                int pre_dp_ik0 = dp_ik0;
                int pre_dp_ik1 = dp_ik1;
                dp_ik0 = max(pre_dp_ik0, pre_dp_ik1 + p);
                dp_ik1 = max(pre_dp_ik1, pre_dp_ik0 - p);
            }
            return dp_ik0;
        }
        vector<int> dp_ik0(k + 1, 0);
        vector<int> dp_ik1(k + 1, INT_MIN);
        for (const auto& p : prices) {
            for(int j = k; j > 0; --j) {
                dp_ik0[j] = max(dp_ik0[j], dp_ik1[j] + p);
                dp_ik1[j] = max(dp_ik1[j], dp_ik0[j - 1] - p);
            } 
        }
        return dp_ik0[k];     
    }
};
```


## 5. 需要冷却期的股票交易

309\. Best Time to Buy and Sell Stock with Cooldown(Medium)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

题目描述：交易之后需要有一天的冷却时间。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int dp_ik0 = 0, pre_dp_ik0 = 0, dp_ik1 = INT_MIN;
        for (const auto& p : prices) {
            int tmp = dp_ik0;
            dp_ik0 = max(dp_ik0, dp_ik1 + p);
            dp_ik1 = max(dp_ik1, pre_dp_ik0 - p);
            pre_dp_ik0 = tmp;
        }
        return dp_ik0;
    }
};
```

## 6. 需要交易费用的股票交易

714\. Best Time to Buy and Sell Stock with Transaction Fee (Medium)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

```html
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1
Selling at prices[3] = 8
Buying at prices[4] = 4
Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

题目描述：每交易一次，都要支付一定的费用。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int dp_ik0 = 0, dp_ik1 = INT_MIN;
        for (const auto& p : prices) {
            int pre_dp_ik0 = dp_ik0;
            int pre_dp_ik1 = dp_ik1;
            dp_ik0 = max(pre_dp_ik0, pre_dp_ik1 + p);
            dp_ik1 = max(pre_dp_ik1, pre_dp_ik0 - p - fee);
        }
        return dp_ik0;
    }
};
```



# 字符串编辑

## 1. 删除两个字符串的字符使它们相等

583\. Delete Operation for Two Strings (Medium)

[Leetcode](https://leetcode.com/problems/delete-operation-for-two-strings/description/) / [力扣](https://leetcode-cn.com/problems/delete-operation-for-two-strings/description/)

```html
Input: "sea", "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```

可以转换为求两个字符串的最长公共子序列问题。

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<int> dp(word2.size() + 1);
        for (int i = 0; i < dp.size(); ++i) 
            dp[i] = i;
        int up_left = 0;
        for (int i = 1; i <= word1.size(); ++i) 
            for (int j = 0; j <= word2.size(); ++j) {
                if (j == 0) {
                    up_left = i - 1; dp[j] = i; continue;
                }
                if (word1[i - 1] == word2[j - 1]) {
                    int tmp = dp[j];
                    dp[j] = up_left;
                    up_left = tmp;
                } else {
                    up_left = dp[j];
                    dp[j] = min(dp[j], dp[j - 1]) + 1;
                }
            }
        return dp.back();
    }
};
```

## 2. 编辑距离

72\. Edit Distance (Hard)

[Leetcode](https://leetcode.com/problems/edit-distance/description/) / [力扣](https://leetcode-cn.com/problems/edit-distance/description/)

```html
Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation:
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation:
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

题目描述：修改一个字符串成为另一个字符串，使得修改次数最少。一次修改操作包括：插入一个字符、删除一个字符、替换一个字符。

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<int> dp(word2.length() + 1, 0);
        int up_left = 0; // holder for dp[up_left]
        for (int i = 0; i <= word1.length(); ++i)
            for (int j = 0; j <= word2.length(); ++j) {
                if (i == 0) {dp[j] = j; continue;}
                if (j == 0) {dp[j] = i; up_left = i - 1; continue;}
                if (word1[i - 1] == word2[j - 1]) {
                    int n = up_left;
                    up_left = dp[j];
                    dp[j] = n;
                } else {
                    int n = 1 + min(up_left, min(dp[j], dp[j - 1]));
                    up_left = dp[j];
                    dp[j] = n;
                }
            }
        return dp.back(); 
    }
};
```

## 3. 复制粘贴字符

650\. 2 Keys Keyboard (Medium)

[Leetcode](https://leetcode.com/problems/2-keys-keyboard/description/) / [力扣](https://leetcode-cn.com/problems/2-keys-keyboard/description/)

题目描述：最开始只有一个字符 A，问需要多少次操作能够得到 n 个字符 A，每次操作可以复制当前所有的字符，或者粘贴。

```
Input: 3
Output: 3
Explanation:
Intitally, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.
```
这题好像dp做并没有省时间。因为1-2年所以不细究了。
```c++
class Solution {
public:
    int minSteps(int n) {
        if (n < 2) return 0;
        int ans = n;
        for (int i = n - 1; i > 1; --i) {
            if (n % i == 0)
                ans = min(ans, minSteps(i) + (n / i));
        }
        return ans;
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
