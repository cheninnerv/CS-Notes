<!-- GFM-TOC -->
* [快速选择](#快速选择)
* [堆](#堆)
    * [1. Kth Element](#1-kth-element)
* [桶排序](#桶排序)
    * [1. 出现频率最多的 k 个元素](#1-出现频率最多的-k-个元素)
    * [2. 按照字符出现次数对字符串排序](#2-按照字符出现次数对字符串排序)
* [荷兰国旗问题](#荷兰国旗问题)
    * [1. 按颜色进行排序](#1-按颜色进行排序)
<!-- GFM-TOC -->


# 快速选择

用于求解   **Kth Element**   问题，也就是第 K 个元素的问题。

可以使用快速排序的 partition() 进行实现。需要先打乱数组，否则最坏情况下时间复杂度为 O(N<sup>2</sup>)。

# 堆

用于求解   **TopK Elements**   问题，也就是 K 个最小元素的问题。可以维护一个大小为 K 的最小堆，最小堆中的元素就是最小元素。最小堆需要使用大顶堆来实现，大顶堆表示堆顶元素是堆中最大元素。这是因为我们要得到 k 个最小的元素，因此当遍历到一个新的元素时，需要知道这个新元素是否比堆中最大的元素更小，更小的话就把堆中最大元素去除，并将新元素添加到堆中。所以我们需要很容易得到最大元素并移除最大元素，大顶堆就能很好满足这个要求。

堆也可以用于求解 Kth Element 问题，得到了大小为 k 的最小堆之后，因为使用了大顶堆来实现，因此堆顶元素就是第 k 大的元素。

快速选择也可以求解 TopK Elements 问题，因为找到 Kth Element 之后，再遍历一次数组，所有小于等于 Kth Element 的元素都是 TopK Elements。

可以看到，快速选择和堆排序都可以求解 Kth Element 和 TopK Elements 问题。

## 1. Kth Element

215\. Kth Largest Element in an Array (Medium)

[Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/description/) / [力扣](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/description/)

```text
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

题目描述：找到倒数第 k 个的元素。

**排序**  ：时间复杂度 O(NlogN)，空间复杂度 O(1)

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() - k];
    }
};
```

**堆**  ：时间复杂度 O(NlogK)，空间复杂度 O(K)。

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> min_heap;
        for (const auto& n : nums) {
            min_heap.push(n);
            if (min_heap.size() > k)
                min_heap.pop();
        }
        return min_heap.top();
    }
};
```

**快速选择**  ：时间复杂度 O(N)，空间复杂度 O(1)

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int l = 0, r = nums.size() - 1;
        while (true) {
            int pos = partition(nums, l, r);
            if (pos == k - 1)
                return nums[pos];
            else if (pos > k - 1)
                r = pos - 1;
            else
                l = pos + 1;
        }
    }
private:
    int partition(vector<int>& nums, int l, int r) {
        int p = l;
        int pivot = nums[l]; l += 1;
        while (l <= r) {
            if (nums[l] < pivot && nums[r] > pivot)
                swap(nums[l++], nums[r--]);
            if (nums[l] >= pivot) l++;
            if (nums[r] <= pivot) r--;
        }
        swap(nums[p], nums[r]);
        return r;
    }
};
```

# 桶排序

## 1. 出现频率最多的 k 个元素

347\. Top K Frequent Elements (Medium)

[Leetcode](https://leetcode.com/problems/top-k-frequent-elements/description/) / [力扣](https://leetcode-cn.com/problems/top-k-frequent-elements/description/)

```html
Given [1,1,1,2,2,3] and k = 2, return [1,2].
```

设置若干个桶，每个桶存储出现频率相同的数。桶的下标表示数出现的频率，即第 i 个桶中存储的数出现的频率为 i, 注意idx要加1（因为出现次数是1based）。

把数都放到桶之后，从后向前遍历桶，最先得到的 k 个数就是出现频率最多的的 k 个数。

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        vector<int> ans;
        vector<vector<int>> bucket(nums.size() + 1);
        for (const auto& n : nums)
            freq[n]++;
        for (const auto& entry : freq)
            bucket[entry.second].push_back(entry.first);
        for (int i = bucket.size() - 1; i >= 0; --i) {
            if (!bucket[i].size()) continue;
            for (int j = 0; j < bucket[i].size(); ++j) {
                ans.push_back(bucket[i][j]);
                if (ans.size() == k)
                    return ans;
            }
        }
        return {};
    }
};
```

## 692 Top K Frequent Words

```html
Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

Example 1:
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
Example 2:
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
Note:
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Input words contain only lowercase letters.
Follow up:
Try to solve it in O(n log k) time and O(n) extra space.
```
```c++
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        vector<string> ans;
        unordered_map<string, int> freq;
        vector<vector<string>> bucket(words.size() + 1);
        for (const auto& w : words)
            freq[w]++;
        for (const auto& entry : freq)
            bucket[entry.second].push_back(entry.first);
        for (int i = bucket.size() - 1; i >= 0; --i) {
            if (bucket[i].size())
                sort(bucket[i].begin(), bucket[i].end());
            for (int j = 0; j < bucket[i].size(); ++j) {
                ans.push_back(bucket[i][j]);
                if (ans.size() == k)
                    return ans;
            }
        }
        return ans;
    }
};
```

## 2. 按照字符出现次数对字符串排序

451\. Sort Characters By Frequency (Medium)

[Leetcode](https://leetcode.com/problems/sort-characters-by-frequency/description/) / [力扣](https://leetcode-cn.com/problems/sort-characters-by-frequency/description/)

```html
Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```
kc: 很重要的影响运行时间的两点： 
1) unordered_map to map is from O(1) to O(logk) k:key.
2) 对计数数组的sort is much faster than 完整的数组（string) is because freq_char's size is much smaller than s.length().

```c++
// 108 ms
class Solution {
public:
    string frequencySort(string s) {
        vector<int> freq(128); // ASCII
        for (const auto& c : s)
            freq[c]++;
        sort(s.begin(), s.end(), [&](int a, int b) {
            return freq[a] > freq[b] || (freq[a] == freq[b] && a > b);
        });
        return s;
    }
};


// 8 ms
class Solution {
public:
    string frequencySort(string s) {
        vector<int> freq(128); // ASCII
        vector<pair<int, char>> freq_char;
        for (const auto& c : s)
            freq[c]++;
        for (int i = 0; i < freq.size(); ++i) {
            if (freq[i] > 0)
                freq_char.push_back(make_pair(freq[i], i));
        }
        // 1) unordered_map to map is from O(1) to O(logk) k:key
        // 2) here the sort is much faster is because freq_char's size is much smaller than s.length()
        sort(freq_char.rbegin(), freq_char.rend());
        string ans;
        for (const auto& p : freq_char)
            ans.append(p.first, p.second);     
        return ans;
    }
};
```

# 荷兰国旗问题

荷兰国旗包含三种颜色：红、白、蓝。

有三种颜色的球，算法的目标是将这三种球按颜色顺序正确地排列。它其实是三向切分快速排序的一种变种，在三向切分快速排序中，每次切分都将数组分成三个区间：小于切分元素、等于切分元素、大于切分元素，而该算法是将数组分成三个区间：等于红色、等于白色、等于蓝色。

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/7a3215ec-6fb7-4935-8b0d-cb408208f7cb.png"/> </div><br>


## 1. 按颜色进行排序

75\. Sort Colors (Medium)

[Leetcode](https://leetcode.com/problems/sort-colors/description/) / [力扣](https://leetcode-cn.com/problems/sort-colors/description/)

```html
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

题目描述：只有 0/1/2 三种颜色。

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        for (int m = 0; m < nums.size(); ++m) {
            while ((nums[m] == 0 && m >= l) || (nums[m] == 2 && m <= r)) {
                if (nums[m] == 0 && m >= l) swap(nums[m], nums[l++]);
                if (nums[m] == 2 && m <= r) swap(nums[m], nums[r--]);
            }
        }
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
