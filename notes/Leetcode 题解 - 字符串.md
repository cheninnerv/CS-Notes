<!-- GFM-TOC -->
* [1. 字符串循环移位包含](#1-字符串循环移位包含)
* [2. 字符串翻转](#2-字符串翻转)
* [3. 最长回文子字符串](#3-最长回文子字符串)
* [3.5 有效回文](#3.5-有效回文)
* [4. 两个字符串包含的字符是否完全相同](#4-两个字符串包含的字符是否完全相同)
* [5. 计算一组字符集合可以组成的回文字符串的最大长度](#5-计算一组字符集合可以组成的回文字符串的最大长度)
* [6. 字符串同构](#6-字符串同构)
* [7. 回文子字符串个数](#7-回文子字符串个数)
* [8. 判断一个整数是否是回文数](#8-判断一个整数是否是回文数)
* [9. 回文切割](#9-回文切割)
* [10. Wordbreak等其他](#9-Wordbreak等其他)
<!-- GFM-TOC -->


# 1. 字符串循环移位包含

[LeetCode 28. Implement strStr()](#)
```html
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

Input: haystack = "hello", needle = "ll"
Output: 2
Example 2:

Input: haystack = "aaaaa", needle = "bba"
Output: -1
Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C’s strstr() and Java’s indexOf().
```

给定两个字符串 s1 和 s2，要求判定 s2 是否能够被 s1 做循环移位得到的字符串包含。

s1 进行循环移位的结果是 s1s1 的子字符串，因此只要判断 s2 是否是 s1s1 的子字符串即可。

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        int h = haystack.length(), n = needle.length();
        for (int i = 0; i <= h - n; ++i) {
            bool found = true;
            for (int j = 0; j < n; ++j) {
                if (haystack[i + j] != needle[j]) {
                    found = false;
                    break;
                }
            }
            if (found)
                return i;
        }
        return -1;
    }
};
```

# 2. 字符串翻转

[344. Reverse String](#)

```html
Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.


Example 1:

Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
Example 2:

Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int l = 0, r = s.size() - 1;
        while (l < r) {
            char temp = s[l];
            s[l++] = s[r];
            s[r--] = temp;
        }
    }
};
```

# 3. 最长回文子字符串

[5. Longest Palindromic Substring](#)

```html
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
Example 2:

Input: "cbbd"
Output: "bb"
```

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        auto GetLen = [&] (int l, int r) {
            while (l >= 0 && r < n && s[l] == s[r]) {
                --l;
                ++r;
            }
            return r - l - 1; // -1 is because l and r are right outside of the palindrome after exiting while loop.
        };
        int start = 0;
        int max_len = 0;
        for (int i = 0; i < n; ++i) {
            int len = max(GetLen(i, i), GetLen(i, i + 1));
            if (len > max_len) {
                max_len = len;
                start = i - (len - 1) / 2;
            }
        }
        return s.substr(start, max_len);
    }
};
```



# 3.5 有效回文

[125. Valid Palindrome](#)

```html
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.

Example 1:

Input: "A man, a plan, a canal: Panama"
Output: true
Example 2:

Input: "race a car"
Output: false
 

Constraints:

s consists only of printable ASCII characters.
```

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int l = 0, r = s.length() - 1;
        while (l < r) {
            if (!isalnum(s[l])) {
                ++l;
                continue;
            }
            if (!isalnum(s[r])) {
                --r;
                continue;
            }
            if (tolower(s[l++]) != tolower(s[r--]))
                return false;
        }
        return true;
    }
};
```


# 4. 两个字符串包含的字符是否完全相同

242\. Valid Anagram (Easy)

[Leetcode](https://leetcode.com/problems/valid-anagram/description/) / [力扣](https://leetcode-cn.com/problems/valid-anagram/description/)

```html
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.
```

可以用 HashMap 来映射字符与出现次数，然后比较两个字符串出现的字符数量是否相同。

由于本题的字符串只包含 26 个小写字符，因此可以使用长度为 26 的整型数组对字符串出现的字符进行统计，不再使用 HashMap。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length())
            return false;
        vector<int> num(26, 0); // 128 covers ASCII, 1,111,998 covers possible Unicode characters.
        for (const char& c : s)
            ++num[c - 'a']; // can't use num[c], because num[c] returns [0~128], num[c - 'a'] returns [0~26]
        for (const char& c : t) {
            if (--num[c - 'a'] < 0)
                return false;
        }
        return true;
    }
};
```

# 5. 计算一组字符集合可以组成的回文字符串的最大长度

409\. Longest Palindrome (Easy)

[Leetcode](https://leetcode.com/problems/longest-palindrome/description/) / [力扣](https://leetcode-cn.com/problems/longest-palindrome/description/)

```html
Input : "abccccdd"
Output : 7
Explanation : One longest palindrome that can be built is "dccaccd", whose length is 7.
```

使用长度为 256 的整型数组来统计每个字符出现的个数，每个字符有偶数个可以用来构成回文字符串。

因为回文字符串最中间的那个字符可以单独出现，所以如果有单独的字符就把它放到最中间。

```c++
class Solution {
public:
    int longestPalindrome(string s) {
        vector<int> num(128, 0); // if input is alphabets then vector of 128 is faster than hashmap
        int ans = 0;
        bool odd = false;
        for (const char& c : s)
            ++num[c];
        for (const int& n : num) {
            ans += (n >> 1) << 1; // this equals to ans += (n / 2) * 2
            if (n % 2 == 1)
                odd = true;
        }
        ans = odd ? ans + 1 : ans;
        return ans;
    }
};
```

# 6. 字符串同构

205\. Isomorphic Strings (Easy)

[Leetcode](https://leetcode.com/problems/isomorphic-strings/description/) / [力扣](https://leetcode-cn.com/problems/isomorphic-strings/description/)

```html
Given "egg", "add", return true.
Given "foo", "bar", return false.
Given "paper", "title", return true.
```

记录一个字符上次出现的位置，如果两个字符串中的字符上次出现的位置一样，那么就属于同构。

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        vector<int> en_s(128, -1), en_t(128, -1);
        for (int i = 0; i < s.length(); ++i) {
            if (en_s[s[i]] != en_t[t[i]])
                return false; 
            en_s[s[i]] = i;
            en_t[t[i]] = i; 
        }
        return true;
    }
};
```

# 7. 回文子字符串个数

647\. Palindromic Substrings (Medium)

[Leetcode](https://leetcode.com/problems/palindromic-substrings/description/) / [力扣](https://leetcode-cn.com/problems/palindromic-substrings/description/)

```html
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

从字符串的某一位开始，尝试着去扩展子字符串。kc: 和上面👆那道Longest Palindromic Substring几乎一样。

```c++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.length();
        auto CountNum = [&] (int l, int r) {
            int count = 0;
            while (l >= 0 && r < n && s[l] == s[r]) {
                ++count;
                --l;
                ++r;
            }
            return count; 
        };
        int max_count = 0;
        for (int i = 0; i < n; ++i) {
            max_count += CountNum(i, i) + CountNum(i, i + 1);
        }
        return max_count;
    }
};
```

# 8. 判断一个整数是否是回文数

9\. Palindrome Number (Easy)

[Leetcode](https://leetcode.com/problems/palindrome-number/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-number/description/)

要求不能使用额外空间，也就不能将整数转换为字符串进行判断。

将整数分成左右两部分，右边那部分需要转置，然后判断这两部分是否相等。

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0) return false;
        int div = 1;
        while (x / div >= 10) div *= 10;
        while (x > 0) {
            if (x / div != x % 10)
                return false;
            x = (x % div) / 10;
            div /= 100;
        }
        return true;
    }
};
```

# 9. 回文切割

131\. Palindrome Partitioning (Medium)

[Leetcode](https://leetcode.com/problems/palindrome-partitioning/description/) / [力扣](https://leetcode-cn.com/problems/palindrome-partitioning/description/)

```html
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example:

Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```

```c++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> ans;
        vector<string> row;
        PartitionHelper(s, 0, row, ans);
        return ans;
    }
private:
    void PartitionHelper(string s, int start, vector<string>& row, vector<vector<string>>& ans) {
        if(start == s.length()) {
            ans.push_back(row);
            return;
        }
        for (int i = start; i < s.length(); ++i) {
            if (!IsPalindrome(s, start, i))
                continue;
            row.push_back(s.substr(start, i - start + 1));
            PartitionHelper(s, i + 1, row, ans);
            row.pop_back();
        }
    }
     
    bool IsPalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end]) return false;
            ++start; --end;
        }
        return true;
    }
};
```
# 10. Wordbreak等其他

139. Word Break
Medium

```html

Share
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.
Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```
```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict_set(wordDict.cbegin(), wordDict.cend());
        return wordBreak(s, dict_set);
    }
private:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
        if (s == "") return true;
        // memory
        if (mem_.count(s)) return mem_[s];
        if (wordDict.count(s)) return mem_[s] = true;
        
        for (size_t i = 1; i < s.length(); ++i) {
            if (wordBreak(s.substr(0, i), wordDict) && 
                wordBreak(s.substr(i, s.length() - i), wordDict)) 
                return mem_[s] = true; 
        }
        return mem_[s] = false;
    }
    
    unordered_map<string, bool> mem_;
};
```


140. Word Break II
```html
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

Note:

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.
Example 1:

Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]
Example 2:

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
Example 3:

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```

```c++
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> word_set(wordDict.cbegin(), wordDict.cend());
        return wordBreak(s, word_set);
    }
    
private:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        if (mem_.count(s)) 
            return mem_[s];
        vector<string> ans;
        if (wordDict.count(s))
            ans.insert(ans.end(), s);
        for (size_t i = 1; i < s.length(); ++i) {
            string word = s.substr(i);
            if (!wordDict.count(word))
                continue;
            vector<string> candidates = AppendWord(wordBreak(s.substr(0, i), wordDict), word);
            ans.insert(ans.end(), candidates.begin(), candidates.end());
        }
        return mem_[s] = ans;
    }
    
    vector<string> AppendWord(vector<string> strs, string word) {
        vector<string> new_strs;
        for (string s : strs) {
            new_strs.push_back(s + " " + word);
        }
        return new_strs;
    }
    
    unordered_map<string, vector<string>> mem_;
};
```




<div align="center"><img width="320px" src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/githubio/公众号二维码-2.png"></img></div>
