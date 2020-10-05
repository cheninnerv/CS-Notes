<!-- GFM-TOC -->
* [1. 求开方](#1-求开方)
* [2. 大于给定元素的最小元素](#2-大于给定元素的最小元素)
* [3. 有序数组的 Single Element](#3-有序数组的-single-element)
* [4. 第一个错误的版本](#4-第一个错误的版本)
* [5. 旋转数组的最小数字](#5-旋转数组的最小数字)
* [6. 查找区间](#6-查找区间)
<!-- GFM-TOC -->


**正常实现**  

```text
Input : [1,2,3,4,5]
key : 3
return the index : 2
```

```java
public int binarySearch(int[] nums, int key) {
    int l = 0, h = nums.length - 1;
    while (l <= h) {
        int m = l + (h - l) / 2;
        if (nums[m] == key) {
            return m;
        } else if (nums[m] > key) {
            h = m - 1;
        } else {
            l = m + 1;
        }
    }
    return -1;
}
```

**时间复杂度**  

二分查找也称为折半查找，每次都能将查找区间减半，这种折半特性的算法时间复杂度为 O(logN)。

**m 计算**  

有两种计算中值 m 的方式：

- m = (l + h) / 2
- m = l + (h - l) / 2

l + h 可能出现加法溢出，也就是说加法的结果大于整型能够表示的范围。但是 l 和 h 都为正数，因此 h - l 不会出现加法溢出问题。所以，最好使用第二种计算法方法。

**未成功查找的返回值**  

循环退出时如果仍然没有查找到 key，那么表示查找失败。可以有两种返回值：

- -1：以一个错误码表示没有查找到 key
- l：将 key 插入到 nums 中的正确位置

**变种**  

二分查找可以有很多变种，实现变种要注意边界值的判断。例如在一个有重复元素的数组中查找 key 的最左位置的实现如下：

```java
public int binarySearch(int[] nums, int key) {
    int l = 0, h = nums.length - 1;
    while (l < h) {
        int m = l + (h - l) / 2;
        if (nums[m] >= key) {
            h = m;
        } else {
            l = m + 1;
        }
    }
    return l;
}
```

该实现和正常实现有以下不同：

- h 的赋值表达式为 h = m
- 循环条件为 l < h
- 最后返回 l 而不是 -1

在 nums[m] >= key 的情况下，可以推导出最左 key 位于 [l, m] 区间中，这是一个闭区间。h 的赋值表达式为 h = m，因为 m 位置也可能是解。

在 h 的赋值表达式为 h = m 的情况下，如果循环条件为 l <= h，那么会出现循环无法退出的情况，因此循环条件只能是 l < h。以下演示了循环条件为 l <= h 时循环无法退出的情况：

```text
nums = {0, 1, 2}, key = 1
l   m   h
0   1   2  nums[m] >= key
0   0   1  nums[m] < key
1   1   1  nums[m] >= key
1   1   1  nums[m] >= key
...
```

当循环体退出时，不表示没有查找到 key，因此最后返回的结果不应该为 -1。为了验证有没有查找到，需要在调用端判断一下返回位置上的值和 key 是否相等。

# 1. 求开方

69\. Sqrt(x) (Easy)

[Leetcode](https://leetcode.com/problems/sqrtx/description/) / [力扣](https://leetcode-cn.com/problems/sqrtx/description/)

```html
Input: 4
Output: 2

Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we want to return an integer, the decimal part will be truncated.
```

一个数 x 的开方 sqrt 一定在 0 \~ x 之间，并且满足 sqrt == x / sqrt。可以利用二分查找在 0 \~ x 之间查找 sqrt。

对于 x = 8，它的开方是 2.82842...，最后应该返回 2 而不是 3。在循环条件为 l <= h 并且循环退出时，h 总是比 l 小 1，也就是说 h = 2，l = 3，因此最后的返回值应该为 h 而不是 l。

```c++
class Solution {
public:
    int mySqrt(int x) {
        if (x <= 1) return x;
        int l = 0, r = x;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (x / m >= m) l = m + 1;
            else r = m;
        }
        return r - 1;
    }
};

// Newton's method
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0) return 0;
        double pre = x / 2.0, ans = 0.0;
        while (true) {
            ans = (pre + x / pre) / 2;
            if (abs(ans - pre) <= 1e-6) return static_cast<int>(ans);
            pre = ans;
        }
        return -1;
    }
};
```

# 2. 大于给定元素的最小元素

744\. Find Smallest Letter Greater Than Target (Easy)

[Leetcode](https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/) / [力扣](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/description/)

```html
Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
```

题目描述：给定一个有序的字符数组 letters 和一个字符 target，要求找出 letters 中大于 target 的最小字符，如果找不到就返回第 1 个字符。

```c++
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int l = 0, r = letters.size(), m = 0;
        while (l < r) {
            m = l + (r - l) / 2;
            if (letters[m] <= target)
                l = m + 1;
            else
                r = m;
        }
        return l == letters.size() ? letters[0] : letters[l];
    }
};
```

# 3. 有序数组的 Single Element

540\. Single Element in a Sorted Array (Medium)

[Leetcode](https://leetcode.com/problems/single-element-in-a-sorted-array/description/) / [力扣](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/description/)

```html
Input: [1, 1, 2, 3, 3, 4, 4, 8, 8]
Output: 2
```

题目描述：一个有序数组只有一个数不出现两次，找出这个数。

要求以 O(logN) 时间复杂度进行求解，因此不能遍历数组并进行异或操作来求解，这么做的时间复杂度为 O(N)。

https://zxi.mytechroad.com/blog/algorithms/binary-search/leetcode-540-single-element-in-a-sorted-array/
核心思想是利用奇偶来判断往左走还是往右走，n是m的parter, 就是mn是一对，正常情况是相等的，所以如果mid相等说明那个数在右边，不然的话在左边。

```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        if (nums.size() < 2) return nums[0];
        int l = 0, r = nums.size(), m = 0, n = 0;
        while (l < r) {
            m = l + (r - l) / 2;
            n = m % 2 ? m - 1: m + 1;
            if (nums[m] == nums[n])
                l = m + 1;
            else
                r = m;
        }
        return nums[l];
    }
};
```

# 4. 第一个错误的版本

278\. First Bad Version (Easy)

[Leetcode](https://leetcode.com/problems/first-bad-version/description/) / [力扣](https://leetcode-cn.com/problems/first-bad-version/description/)

题目描述：给定一个元素 n 代表有 [1, 2, ..., n] 版本，在第 x 位置开始出现错误版本，导致后面的版本都错误。可以调用 isBadVersion(int x) 知道某个版本是否错误，要求找到第一个错误的版本。

```c++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n, m = 1;
        while (l < r) {
            m = l + (r - l) / 2;
            if (isBadVersion(m))
                r = m;
            else
                l = m + 1;
        }
        return l;
    }
};
```

# 5. 旋转数组的最小数字

153\. Find Minimum in Rotated Sorted Array (Medium)

[Leetcode](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/) / [力扣](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/description/)

```html
Input: [3,4,5,1,2],
Output: 1
```
kc: 自己写的，就是寻找这个开始变的数（inflection point）, 方法是和第一个数nums[0]比较，如果大于nums[0]则这个数在右边，否则在左边。trick的地方是可以在[1，n]的下标区间寻找这个数，找到了就是返回它，没找到l==size()的话就说明没有这个数那么返回nums[0]即可。

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 1, r = nums.size(), m = 0;
        while (l < r) {
            m = l + (r - l) / 2;
            if (nums[m] > nums[0])
                l = m + 1;
            else
                r = m;
        }
        return l == nums.size() ? nums[0] : nums[l];
    }
};
```

# 6. 查找区间

34\. Find First and Last Position of Element in Sorted Array

[Leetcode](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) / [力扣](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```html
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

题目描述：给定一个有序数组 nums 和一个目标 target，要求找到 target 在 nums 中的第一个位置和最后一个位置。

可以用二分查找找出第一个位置和最后一个位置，但是寻找的方法有所不同，需要实现两个二分查找。

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = 0, r = nums.size(), m = 0;
        vector<int> ans;
        while (l < r) {
            m = l + (r - l) / 2;
            if (nums[m] < target)
                l = m + 1;
            else
                r = m;
        }
        if (nums.empty() || l == nums.size() || nums[l] != target) 
            return {-1, -1};
        ans.push_back(l);
        l = 0, r = nums.size(), m = 0;
        while (l < r) {
            m = l + (r - l) / 2;
            if (nums[m] <= target)
                l = m + 1;
            else
                r = m;
        }
        ans.push_back(l - 1);
        return ans;
    }
};
```





<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
