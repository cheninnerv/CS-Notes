<!-- GFM-TOC -->
* [1. 把数组中的 0 移到末尾](#1-把数组中的-0-移到末尾)
* [2. 改变矩阵维度](#2-改变矩阵维度)
* [3. 找出数组中最长的连续 1](#3-找出数组中最长的连续-1)
* [4. 有序矩阵查找](#4-有序矩阵查找)
* [5. 有序矩阵的 Kth Element](#5-有序矩阵的-kth-element)
* [6. 一个数组元素在 [1, n] 之间，其中一个数被替换为另一个数，找出重复的数和丢失的数](#6-一个数组元素在-[1,-n]-之间，其中一个数被替换为另一个数，找出重复的数和丢失的数)
* [7. 找出数组中重复的数，数组值在 [1, n] 之间](#7-找出数组中重复的数，数组值在-[1,-n]-之间)
* [8. 数组相邻差值的个数](#8-数组相邻差值的个数)[Deleted by KC]
* [9. 数组的度](#9-数组的度)
* [10. 对角元素相等的矩阵](#10-对角元素相等的矩阵)
* [11. 嵌套数组](#11-嵌套数组)
* [12. 分隔数组](#12-分隔数组)
<!-- GFM-TOC -->


# 1. 把数组中的 0 移到末尾

283\. Move Zeroes (Easy)

[Leetcode](https://leetcode.com/problems/move-zeroes/description/) / [力扣](https://leetcode-cn.com/problems/move-zeroes/description/)

```html
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
```

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        // l and r either move togather or l stay at the leftest zero, and r move to the next non-zero
        size_t l = 0, r = 0;
        while (r < nums.size()) {
            if (nums[r] != 0) {
                swap(nums[l], nums[r]); // nothing happens when l and r point to the same index.
                l++;
            }
            r++;
        }
    }
};
```

# 2. 改变矩阵维度

566\. Reshape the Matrix (Easy)

[Leetcode](https://leetcode.com/problems/reshape-the-matrix/description/) / [力扣](https://leetcode-cn.com/problems/reshape-the-matrix/description/)

```html
Input:
nums =
[[1,2],
 [3,4]]
r = 1, c = 4

Output:
[[1,2,3,4]]

Explanation:
The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
```

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        if (nums.size() == 0) return nums;
        int old_r = nums.size();
        int old_c = nums[0].size();
        if (r * c != old_r * old_c) return nums;
        
        vector<vector<int>> ans(r, vector<int>(c));
        for (size_t i = 0; i < old_r * old_c; ++i) {
            int old_m = i / old_c;
            int old_n = i % old_c;
            int m = i / c;
            int n = i % c; 
            ans[m][n] = nums[old_m][old_n];
        }
        return ans;
    }
};
```

# 3. 找出数组中最长的连续 1

485\. Max Consecutive Ones (Easy)

[Leetcode](https://leetcode.com/problems/max-consecutive-ones/description/) / [力扣](https://leetcode-cn.com/problems/max-consecutive-ones/description/)

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int ans = 0, cnt = 0;
        for (size_t i = 0; i < nums.size(); ++i) {
            if (nums[i] == 0) cnt = 0;
            if (nums[i] == 1) cnt++;
            if (cnt > ans) ans = cnt;
        }
        return ans;
    }
};
```

# 4. 有序矩阵查找

240\. Search a 2D Matrix II (Medium)

[Leetcode](https://leetcode.com/problems/search-a-2d-matrix-ii/description/) / [力扣](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/description/)

```html
[
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
]
```

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int r = 0, c = matrix[0].size() - 1;
        while (c >= 0 && r < matrix.size()) {
            if (matrix[r][c] == target)
                return true;
            else if (matrix[r][c] < target)
                r++;
            else
                c--;
        }
        return false;
    }
};
```

74\. Search a 2D Matrix (Medium)

[Leetcode](https://leetcode.com/problems/search-a-2d-matrix/description/) / [力扣](https://leetcode-cn.com/problems/search-a-2d-matrix/description/)

```html
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
Example 1:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
Example 2:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() || matrix[0].empty()) return false;
        int l = 0, r = matrix.size() * matrix[0].size();
        int c = matrix[0].size();
        while (l < r) {
            int m = l + (r - l) / 2;
            int m_val = matrix[m / c][m % c];
            if (m_val == target)
                return true;
            else if (m_val > target)
                r = m;
            else
                l = m + 1;
        }
        return false;
    }
};
```

# 5. 有序矩阵的 Kth Element

378\. Kth Smallest Element in a Sorted Matrix ((Medium))

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/description/)

```html
matrix = [
  [ 1,  5,  9],
  [10, 11, 13],
  [12, 13, 15]
],
k = 8,

return 13.
```

解题：二分法精髓是判断往左还是往右，这道题用二分法把最大和最小值用l=mid+1, r=mid这种一次只在数值上（+1）的小操作（当然还有本身的二分切割）收敛到要求的值（Kth最小），
所以只要判断往左还是往右，最后总能收敛到。那么怎么判断是往左还是右呢？就用到了cnt, 因为每一行都要单独算有多少个item小于mid_val, 最后cnt的数目代表了总共有多少个item小于mid_val，cnt多于k则说明小的个数多了，target的数值一定小于mid_val, 则一定往左走。往右同理。

二分查找解法：

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int c = matrix[0].size();
        int small_val = matrix[0][0];
        int big_val = matrix.back().back();
        while (small_val < big_val) {
            int cnt = 0;
            int mid_val = small_val + (big_val - small_val) / 2;
            for (auto& row : matrix) {
                cnt += upper_bound(row.begin(), row.end(), mid_val) - row.begin(); // note:upper_bound returns iterator, so must to do "- row.begin()" to get int
            }
            if (cnt < k)
                small_val = mid_val + 1;
            else
                big_val = mid_val;
        }
        return small_val;
    }
};
```

堆解法：
https://www.cnblogs.com/grandyang/p/5727892.html 解法1

# 6. 一个数组元素在 [1, n] 之间，其中一个数被替换为另一个数，找出重复的数和丢失的数

645\. Set Mismatch (Easy)

[Leetcode](https://leetcode.com/problems/set-mismatch/description/) / [力扣](https://leetcode-cn.com/problems/set-mismatch/description/)

```html
Input: nums = [1,2,2,4]
Output: [2,3]
```

```html
Input: nums = [1,2,2,4]
Output: [2,3]
```
yy: https://www.cnblogs.com/grandyang/p/7324242.html 解法二
精髓在于每个index存的数字应该比index大1，index 0 存1，index1存2，以此类推。那就可以用当前数字推出对应的index. 遍历每个数字，然后将其应该出现的位置上的数字变为其相反数，这样如果我们在变为其相反数之前已经成负数了，说明该数字是重复数，将其将入结果res中，然后再用index遍历原数组，如果某个位置上的数字为正数，说明该位置对应的数字没有出现过，加入res中即可.

想到这个方法还是挺难的, 另外for (const auto& val : nums) 这样的写法，前面如果修改了后面nums[x]的值，则val值是会变成修改后的值得，比如遍历1， 2， 3 如果你在遍历1的时候nums[2] *= -1; 则会发生的是，遍历到3的时候val不是3而是-3. 所以重点来了： 一定要加绝对值 abs(val) - 1.

```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> ans;
        for (const auto& val : nums) {
            if (nums[abs(val) - 1] < 0)
                ans.push_back(abs(val));
            else
                nums[abs(val) - 1] *= -1;
        }
        for (int i = 0; i < nums.size(); ++i) {
            // This index hasn't been updated means missing the corresponding number.
            if (nums[i] > 0) // > 0 is because input is 1 ~ n
                ans.push_back(i + 1);
        }
        return ans;
    }
};
```

# 7. 找出数组中重复的数，数组值在 [1, n] 之间

287\. Find the Duplicate Number (Medium)

[Leetcode](https://leetcode.com/problems/find-the-duplicate-number/description/) / [力扣](https://leetcode-cn.com/problems/find-the-duplicate-number/description/)

要求不能修改数组，也不能使用额外的空间。

二分查找解法：

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int l = 0, r = nums.size();
        while (l < r) {
            int m = l + (r - l) / 2;
            int cnt = 0;
            for (const auto& num : nums) {
                if (num <= m)
                    cnt++; // if normal then cnt should <= m, no matter what m, otherwise whichever break this rule is the duplicate (ans), use m to judge and r/l to converge to the ans.
            }
            if (cnt > m)
                r = m;
            else
                l = m + 1;
        }
        return l;
    }
};
```

双指针解法，类似于有环链表中找出环的入口：
我懂了但没写，贴上答案：
```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0, t = 0;
        while (true) {
            slow = nums[slow];
            fast = nums[nums[fast]];
            if (slow == fast) break;
        }
        while (true) {
            slow = nums[slow];
            t = nums[t];
            if (slow == t) break;
        }
        return slow;
    }
};
```


# 9. 数组的度

697\. Degree of an Array (Easy)

[Leetcode](https://leetcode.com/problems/degree-of-an-array/description/) / [力扣](https://leetcode-cn.com/problems/degree-of-an-array/description/)

```html
Input: [1,2,2,3,1,4,2]
Output: 6
```

题目描述：数组的度定义为元素出现的最高频率，例如上面的数组度为 3。要求找到一个最小的子数组，这个子数组的度和原数组一样。

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        unordered_map<int, vector<int>> indices;
        int degree = 0, ans = nums.size();
        // build num to all its indices map
        for (size_t i = 0; i < nums.size(); ++i) {
            indices[nums[i]].push_back(i);
            // calculate the degree
            degree = max(degree, static_cast<int>(indices[nums[i]].size()));
        }
        
        // calculate the min length that has the same degree
        for (const auto& item : indices) {
            if (item.second.size() == degree) {
                ans = min(ans, item.second.back() - item.second.front() + 1);
            }
        }
        return ans;
    }
};
```

# 10. 对角元素相等的矩阵

766\. Toeplitz Matrix (Easy)

[Leetcode](https://leetcode.com/problems/toeplitz-matrix/description/) / [力扣](https://leetcode-cn.com/problems/toeplitz-matrix/description/)

```html
1234
5123
9512

In the above grid, the diagonals are "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]", and in each diagonal all elements are the same, so the answer is True.
```

```c++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) return false;
        for (size_t i = 0; i < matrix.size() - 1; ++i) {
            for (size_t j = 0; j < matrix[0].size() - 1; ++j)
                if(matrix[i][j] != matrix[i + 1][j + 1]) return false;
        }
        return true;
    }
};
```

# 11. 嵌套数组

565\. Array Nesting (Medium)

[Leetcode](https://leetcode.com/problems/array-nesting/description/) / [力扣](https://leetcode-cn.com/problems/array-nesting/description/)

```html
Input: A = [5,4,0,3,1,6,2]
Output: 4
Explanation:
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
```

题目描述：S[i] 表示一个集合，集合的第一个元素是 A[i]，第二个元素是 A[A[i]]，如此嵌套下去。求最大的 S[i]。

```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        unordered_set<int> visited;
        int ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int idx = i;
            int cnt = 0;
            while (!visited.count(idx)) {
                visited.emplace(idx);
                idx = nums[idx];
                cnt++;
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

# 12. 分隔数组

769\. Max Chunks To Make Sorted (Medium)

[Leetcode](https://leetcode.com/problems/max-chunks-to-make-sorted/description/) / [力扣](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/description/)

```html
Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.
```

题目描述：分隔数组，使得对每部分排序后数组就为有序。

```c++
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        int ans = 0;
        for (int i = 0; i < arr.size(); ++i) {
            int nearest_cut_idx = arr[i];
            for (int j = i; j <= nearest_cut_idx; ++j) {
                nearest_cut_idx = max(nearest_cut_idx, arr[j]);
                if (nearest_cut_idx >= arr.back())
                    return ++ans;
                i = j;
            }
            ++ans;
        }
        return ans;
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
