<!-- GFM-TOC -->
* [1. 有序数组的 Two Sum](#1-有序数组的-two-sum)
* [2. 两数平方和](#2-两数平方和)
* [3. 反转字符串中的元音字符](#3-反转字符串中的元音字符)
* [4. 回文字符串](#4-回文字符串)
* [5. 归并两个有序数组](#5-归并两个有序数组)
* [6. 判断链表是否存在环](#6-判断链表是否存在环)
* [7. 最长子序列](#7-最长子序列)
<!-- GFM-TOC -->


双指针主要用于遍历数组，两个指针指向不同的元素，从而协同完成任务。

# 1. 有序数组的 Two Sum

167\. Two Sum II - Input array is sorted (Easy)

[Leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/) / [力扣](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/description/)

```html
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2
```

题目描述：在有序数组中找出两个数，使它们的和为 target。

使用双指针，一个指针指向值较小的元素，一个指针指向值较大的元素。指向较小元素的指针从头向尾遍历，指向较大元素的指针从尾向头遍历。


```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int l = 0, r = numbers.size() - 1;
        while (l < r) {
            if (numbers[l] + numbers[r] == target) 
                return {++l, ++r};
            if (numbers[l] + numbers[r] < target)
                ++l;
            else 
                --r;
        }
        return {};
    }
};
```

# 2. 两数平方和

633\. Sum of Square Numbers (Easy)

[Leetcode](https://leetcode.com/problems/sum-of-square-numbers/description/) / [力扣](https://leetcode-cn.com/problems/sum-of-square-numbers/description/)

```html
Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
```

题目描述：判断一个非负整数是否为两个整数的平方和。

可以看成是在元素为 0\~target 的有序数组中查找两个数，使得这两个数的平方和为 target，如果能找到，则返回 true，表示 target 是两个整数的平方和。

本题和 167\. Two Sum II - Input array is sorted 类似，只有一个明显区别：一个是和为 target，一个是平方和为 target。本题同样可以使用双指针得到两个数，使其平方和为 target。

本题的关键是右指针的初始化，实现剪枝，从而降低时间复杂度。设右指针为 x，左指针固定为 0，为了使 0<sup>2</sup> + x<sup>2</sup> 的值尽可能接近 target，我们可以将 x 取为 sqrt(target)。

因为最多只需要遍历一次 0\~sqrt(target)，所以时间复杂度为 O(sqrt(target))。又因为只使用了两个额外的变量，因此空间复杂度为 O(1)。

```c++
class Solution {
public:
    bool judgeSquareSum(int c) {
        int m = sqrt(c);
        for (int a = 0; a <= m; ++a) {
            int b = sqrt(c - a * a);
            if (a * a + b * b == c) return true;
        }
        return false;
    }
};
```

# 3. 反转字符串中的元音字符

345\. Reverse Vowels of a String (Easy)

[Leetcode](https://leetcode.com/problems/reverse-vowels-of-a-string/description/) / [力扣](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/description/)

```html
Given s = "leetcode", return "leotcede".
```

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/a7cb8423-895d-4975-8ef8-662a0029c772.png" width="400px"> </div><br>

使用双指针，一个指针从头向尾遍历，一个指针从尾到头遍历，当两个指针都遍历到元音字符时，交换这两个元音字符。

为了快速判断一个字符是不是元音字符，我们将全部元音字符添加到集合 HashSet 中，从而以 O(1) 的时间复杂度进行该操作。

- 时间复杂度为 O(N)：只需要遍历所有元素一次
- 空间复杂度 O(1)：只需要使用两个额外变量

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/ef25ff7c-0f63-420d-8b30-eafbeea35d11.gif" width="400px"> </div><br>

```c++
class Solution {
public:
    string reverseVowels(string s) {
      int i = 0;
      int j = s.size() - 1;
      while (i < j) {
        while (!isVowel(s[i]) && i < j) ++i;
        while (!isVowel(s[j]) && i < j) --j;                
        swap(s[i++], s[j--]);
      }
      return s;
    }
private:
  bool isVowel(char c) {
    c = tolower(c);
    return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
  }
};
```

# 4. 回文字符串

680\. Valid Palindrome II (Easy)

[Leetcode](https://leetcode.com/problems/valid-palindrome-ii/description/) / [力扣](https://leetcode-cn.com/problems/valid-palindrome-ii/description/)

```html
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

题目描述：可以删除一个字符，判断是否能构成回文字符串。



```c++
class Solution {
public:
    bool validPalindrome(string s) {
        int l = 0, r = s.length() - 1;
        while (l < r) {
            if (s.at(l) != s.at(r)) 
                return IsPalindrome(s, l + 1, r) || IsPalindrome(s, l, r - 1);
            l++;
            r--;
        }
        return true;
    }
private:
    bool IsPalindrome(string s, int l, int r) {
        while (l < r) {
            if (s.at(l++) != s.at(r--)) return false;
        }
        return true;
    }
};
```

# 5. 归并两个有序数组

88\. Merge Sorted Array (Easy)

[Leetcode](https://leetcode.com/problems/merge-sorted-array/description/) / [力扣](https://leetcode-cn.com/problems/merge-sorted-array/description/)

```html
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

题目描述：把归并结果存到第一个数组上。

需要从尾开始遍历，否则在 nums1 上归并得到的值会覆盖还未进行归并比较的值。

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        m--;
        n--;
        int end = nums1.size() - 1;
        while (m >= 0 && n >= 0) {
            if (nums1[m] >= nums2[n])
                nums1[end--] = nums1[m--];
            else
                nums1[end--] = nums2[n--];     
        }
        while (n >= 0)
            nums1[end--] = nums2[n--];            
    }
};
```

# 6. 判断链表是否存在环

141\. Linked List Cycle (Easy)

[Leetcode](https://leetcode.com/problems/linked-list-cycle/description/) / [力扣](https://leetcode-cn.com/problems/linked-list-cycle/description/)

使用双指针，一个指针每次移动一个节点，一个指针每次移动两个节点，如果存在环，那么这两个指针一定会相遇。

```c++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;
        while (fast && fast->next && slow->next) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) return true;
        }
        return false;
    }
};
```

# 7. 最长子序列

524\. Longest Word in Dictionary through Deleting (Medium)

[Leetcode](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/) / [力扣](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/description/)

```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output:
"apple"
```

题目描述：删除 s 中的一些字符，使得它构成字符串列表 d 中的一个字符串，找出能构成的最长字符串。如果有多个相同长度的结果，返回字典序的最小字符串。

通过删除字符串 s 中的一个字符能得到字符串 t，可以认为 t 是 s 的子序列，我们可以使用双指针来判断一个字符串是否为另一个字符串的子序列。

```c++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        string longest;
        for (const auto& word : d) {
            int i = 0;
            for (const auto& c_str : s) {
                if (c_str == word[i]) {
                    i++;
                }
            }
            if (i == word.length() && 
                (word.length() > longest.length() ||
                (word.length() == longest.length() && word < longest)))
                longest = word;
        }
        return longest;
    }
};
```






<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
