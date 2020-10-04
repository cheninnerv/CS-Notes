<!-- GFM-TOC -->
* [1. 分配饼干](#1-分配饼干)
* [2. 不重叠的区间个数](#2-不重叠的区间个数)
* [3. 投飞镖刺破气球](#3-投飞镖刺破气球)
* [4. 根据身高和序号重组队列](#4-根据身高和序号重组队列)
* [5. 买卖股票最大的收益](#5-买卖股票最大的收益)
* [6. 买卖股票的最大收益 II](#6-买卖股票的最大收益-ii)
* [7. 种植花朵](#7-种植花朵)
* [8. 判断是否为子序列](#8-判断是否为子序列)
* [9. 修改一个数成为非递减数组](#9-修改一个数成为非递减数组)
* [10. 子数组最大的和](#10-子数组最大的和)
* [11. 分隔字符串使同种字符出现在一起](#11-分隔字符串使同种字符出现在一起)
<!-- GFM-TOC -->


保证每次操作都是局部最优的，并且最后得到的结果是全局最优的。

# 1. 分配饼干

455\. Assign Cookies (Easy)

[Leetcode](https://leetcode.com/problems/assign-cookies/description/) / [力扣](https://leetcode-cn.com/problems/assign-cookies/description/)

```html
Input: grid[1,3], size[1,2,4]
Output: 2
```

题目描述：每个孩子都有一个满足度 grid，每个饼干都有一个大小 size，只有饼干的大小大于等于一个孩子的满足度，该孩子才会获得满足。求解最多可以获得满足的孩子数量。

1. 给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。
2. 因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。

在以上的解法中，我们只在每次分配时饼干时选择一种看起来是当前最优的分配方法，但无法保证这种局部最优的分配方法最后能得到全局最优解。我们假设能得到全局最优解，并使用反证法进行证明，即假设存在一种比我们使用的贪心策略更优的最优策略。如果不存在这种最优策略，表示贪心策略就是最优策略，得到的解也就是全局最优解。

证明：假设在某次选择中，贪心策略选择给当前满足度最小的孩子分配第 m 个饼干，第 m 个饼干为可以满足该孩子的最小饼干。假设存在一种最优策略，可以给该孩子分配第 n 个饼干，并且 m < n。我们可以发现，经过这一轮分配，贪心策略分配后剩下的饼干一定有一个比最优策略来得大。因此在后续的分配中，贪心策略一定能满足更多的孩子。也就是说不存在比贪心策略更优的策略，即贪心策略就是最优策略。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/e69537d2-a016-4676-b169-9ea17eeb9037.gif" width="430px"> </div><br>

```c++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int ans = 0;
        for (int i = 0, j = 0; i < g.size() && j < s.size(); ++j) {
            if (g[i] <= s[j]) {
                ans++;
                i++;
            }
        }
        return ans;
    }
};
```

# 2. 不重叠的区间个数

435\. Non-overlapping Intervals (Medium)

[Leetcode](https://leetcode.com/problems/non-overlapping-intervals/description/) / [力扣](https://leetcode-cn.com/problems/non-overlapping-intervals/description/)

```html
Input: [ [1,2], [1,2], [1,2] ]

Output: 2

Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.
```

```html
Input: [ [1,2], [2,3] ]

Output: 0

Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

题目描述：计算让一组区间不重叠所需要移除的区间个数。

kc: 贪心的概念就是选局部最优解! 两个重叠区间谁end越大越容易和后面overlap，于是无脑删end大的那个。
另外，vector<pair<int, int>> vector<vector<int>>都会按第一个int元素去sort吗？

```c++
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        int ans = 0, l = 0, r = 1;
        while (r < intervals.size()) {
            // if overlapped, compare l and r, and delete the one with bigger end.
            if (intervals[l][1] > intervals[r][0]) {
                l = intervals[l][1] > intervals[r][1] ? r : l;
                r++;
                ans++;
            } else {
                l = r;
                r++;
            }
        }
        return ans;
    }
};
```


# 3. 投飞镖刺破气球

452\. Minimum Number of Arrows to Burst Balloons (Medium)

[Leetcode](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/) / [力扣](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)

```
Input:
[[10,16], [2,8], [1,6], [7,12]]

Output:
2
```

题目描述：气球在一个水平数轴上摆放，可以重叠，飞镖垂直投向坐标轴，使得路径上的气球都被刺破。求解最小的投飞镖次数使所有气球都被刺破。

也是计算不重叠的区间个数，不过和 Non-overlapping Intervals 的区别在于，[1, 2] 和 [2, 3] 在本题中算是重叠区间。

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.empty()) return 0;
        sort(points.begin(), points.end());
        int l_end = points[0][1], r = 1, ans = 1;
        while (r < points.size()) {
            if (l_end >= points[r][0]) {
                l_end = min(l_end, points[r][1]);
                r++;
            } else {
                ans++;
                l_end = points[r][1];
                r++;
            }
        }
        return ans;
    }
};
```

# 4. 根据身高和序号重组队列

406\. Queue Reconstruction by Height(Medium)

[Leetcode](https://leetcode.com/problems/queue-reconstruction-by-height/description/) / [力扣](https://leetcode-cn.com/problems/queue-reconstruction-by-height/description/)

```html
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

题目描述：一个学生用两个分量 (h, k) 描述，h 表示身高，k 表示排在前面的有 k 个学生的身高比他高或者和他一样高。

为了使插入操作不影响后续的操作，身高较高的学生应该先做插入操作，否则身高较小的学生原先正确插入的第 k 个位置可能会变成第 k+1 个位置。

身高 h 降序、个数 k 值升序，然后将某个学生插入队列的第 k 个位置中。

```c++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), [&] (vector<int> a, vector<int> b) {
            return a[0] > b[0] || (a[0] == b[0] && a[1] < b[1]);
        });
        for (int i = 0; i < people.size(); ++i) {
            auto p_copy = people[i];
            people.erase(people.begin() + i);
            people.insert(people.begin() + p_copy[1], p_copy);
        }
        return people;
    }
};
```

# 5. 买卖股票最大的收益

121\. Best Time to Buy and Sell Stock (Easy)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)

题目描述：一次股票交易包含买入和卖出，只进行一次交易，求最大收益。

只要一直记录前面的最小价格，然后不断将当前的价格作为售出价格比较，查看当前收益是不是最大收益。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min_so_far = INT_MAX, ans = 0;
        for (int i = 0; i < prices.size(); ++i) {
            min_so_far = min(min_so_far, prices[i]);
            ans = max(ans, prices[i] - min_so_far);
        }
        return ans;
    }
};
```


# 6. 买卖股票的最大收益 II

122\. Best Time to Buy and Sell Stock II (Easy)

[Leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/) / [力扣](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/description/)

题目描述：可以进行多次交易，多次交易之间不能交叉进行，可以进行多次交易。

对于 [a, b, c, d]，如果有 a <= b <= c <= d ，那么最大收益为 d - a。而 d - a = (d - c) + (c - b) + (b - a) ，这题换句话说就是，股票涨的时候就买(prices[i] < prices[i + 1])，跌的时候不买。这样收益最大。还是greedy. 因此当访问到一个 prices[i] 且 prices[i] - prices[i-1] > 0，那么就把 prices[i] - prices[i-1] 添加到收益中。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans = 0;
        for (int i = 0; i < prices.size() - 1; ++i)
            ans += prices[i] < prices[i + 1] ? prices[i + 1] - prices[i] : 0;
        return ans;
    }
};
```


# 7. 种植花朵

605\. Can Place Flowers (Easy)

[Leetcode](https://leetcode.com/problems/can-place-flowers/description/) / [力扣](https://leetcode-cn.com/problems/can-place-flowers/description/)

```html
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```

题目描述：flowerbed 数组中 1 表示已经种下了花朵。花朵之间至少需要一个单位的间隔，求解是否能种下 n 朵花。
遍历每个index，如果当前index对应的数字是1，就跳过，否则看其前一位和后一位是否都是0，如果是的话就可以种一朵花，并且要把当前位的数字变成1。

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        flowerbed.insert(flowerbed.begin(), 0);
        flowerbed.push_back(0);
        for (int i = 1; i < flowerbed.size() - 1; ++i) {
            if (flowerbed[i] == 0) {
                if (flowerbed[i - 1] == 0 && flowerbed[i + 1] == 0) {
                    flowerbed[i] = 1;
                    n--;
                }
            }     
        }
        return n <= 0;
    }
};
```

# 8. 判断是否为子序列

392\. Is Subsequence (Medium)

[Leetcode](https://leetcode.com/problems/is-subsequence/description/) / [力扣](https://leetcode-cn.com/problems/is-subsequence/description/)

```html
s = "abc", t = "ahbgdc"
Return true.
```
题目中的 Follow up 说如果有大量的字符串s，问我们如何进行优化。那么既然字符串t始终保持不变，我们就可以在t上做一些文章。子序列虽然不需要是连着的子串，但是字符之间的顺序是需要的，那么我们可以建立字符串t中的每个字符跟其位置直接的映射，由于t中可能会出现重复字符，所以把相同的字符出现的所有位置按顺序加到一个数组中，所以就是用 HashMap 来建立每个字符和其位置数组之间的映射。然后遍历字符串s中的每个字符，对于每个遍历到的字符c，我们到 HashMap 中的对应的字符数组中去搜索，由于位置数组是有序的，我们使用二分搜索来加快搜索速度，这里需要注意的是，由于子序列是有顺序要求的，所以需要一个变量 pre 来记录当前匹配到t字符串中的位置，对于当前s串中的字符c，即便在t串中存在，但是若其在位置 pre 之前，也是不能匹配的。所以我们可以使用 uppper_bound() 来二分查找第一个大于 pre 的位置，若不存在，直接返回 false，否则将 pre 更新为二分查找的结果并继续循环即可，参见代码如下：
```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.empty()) return true;
        int s_idx = 0, t_idx = 0;
        for (int t_idx = 0; t_idx < t.length(); ++t_idx) {
            if (s_idx == s.length()) return true;
            if (t.at(t_idx) == s.at(s_idx))
                s_idx++;
        }
        return s_idx == s.length();
    }
};

// follow up
class Solution {
public:
    bool isSubsequence(string s, string t) {
        unordered_map<char, vector<int>> char2idxes;
        int pre = -1;
        for (int i = 0; i < t.length(); ++i) 
            char2idxes[t.at(i)].push_back(i);
        for (int j = 0; j < s.length(); ++j) {
            auto it = upper_bound(char2idxes[s.at(j)].begin(), char2idxes[s.at(j)].end(), pre);
            if (it == char2idxes[s.at(j)].end())
                return false;
            pre = *it;
        }
        return true;
    }
};
```

# 9. 修改一个数成为非递减数组

665\. Non-decreasing Array (Easy)

[Leetcode](https://leetcode.com/problems/non-decreasing-array/description/) / [力扣](https://leetcode-cn.com/problems/non-decreasing-array/description/)

```html
Input: [4,2,3]
Output: True
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

题目描述：判断一个数组是否能只修改一个数就成为非递减数组。

在出现 nums[i] < nums[i - 1] 时，需要考虑的是应该修改数组的哪个数，使得本次修改能使 i 之前的数组成为非递减数组，并且   **不影响后续的操作**  。优先考虑令 nums[i - 1] = nums[i]，因为如果修改 nums[i] = nums[i - 1] 的话，那么 nums[i] 这个数会变大，就有可能比 nums[i + 1] 大，从而影响了后续操作。还有一个比较特别的情况就是 nums[i] < nums[i - 2]，修改 nums[i - 1] = nums[i] 不能使数组成为非递减数组，只能修改 nums[i] = nums[i - 1]。

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int cnt = 1;
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i - 1] > nums[i]) {
                if (cnt <= 0) return false;
                if (i - 2 >= 0 && nums[i - 2] > nums[i])
                    nums[i] = nums[i - 1];
                // no need for else
                cnt--;
            }
        }
        return true;
    }
};
```



# 10. 子数组最大的和

53\. Maximum Subarray (Easy)

[Leetcode](https://leetcode.com/problems/maximum-subarray/description/) / [力扣](https://leetcode-cn.com/problems/maximum-subarray/description/)

```html
For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.
```

In follow up, 题目还要求我们用分治法 Divide and Conquer Approach 来解，这个分治法的思想就类似于二分搜索法，需要把数组一分为二，分别找出左边和右边的最大子数组之和，然后还要从中间开始向左右分别扫描，求出的最大值分别和左右两边得出的最大值相比较取最大的那一个.

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre_max = nums[0], ans = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            pre_max = max(pre_max + nums[i], nums[i]); 
            ans = max(ans, pre_max);
        }
        return ans;
    }
};

// follow up 
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        return DivideConquer(nums, 0, nums.size() - 1);
    }

private:
    int DivideConquer(vector<int>& nums, int l, int r) {
        if (l >= r) return nums[l];
        int m = l + (r - l) / 2;
        int l_max = DivideConquer(nums, l, m - 1);
        int r_max = DivideConquer(nums, m + 1, r);
        int m_val = nums[m], m_max = nums[m];
        for (int i = m - 1; i >= l; --i) {
            m_val += nums[i];
            m_max = max(m_max, m_val);
        }
        m_val = m_max;
        for (int i = m + 1; i <= r; ++i) {
            m_val += nums[i];
            m_max = max(m_max, m_val);
        }
        return max(m_max, max(l_max, r_max));
    }
};
```

# 11. 分隔字符串使同种字符出现在一起

763\. Partition Labels (Medium)

[Leetcode](https://leetcode.com/problems/partition-labels/description/) / [力扣](https://leetcode-cn.com/problems/partition-labels/description/)

```html
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

这题和56 Merge Intervals 一样是青蛙跳跃题，往后跳那类。

```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        unordered_map<char, int> last_idx;
        vector<int> ans;
        int begin = 0, end = 0;
        for (int i = 0; i < S.length(); ++i)
            last_idx[S.at(i)] = i;
        for (int i = 0; i < S.length(); ++i) {
            end = max(end, last_idx[S.at(i)]);
            if (i == end) {
                ans.push_back(end - begin + 1);
                begin = end + 1;
            }
        }
        return ans;
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
