---
title: 'LeetCode 每日一题(4)'
date: 2020-06-15 17:14:40
tags: []
---
# [题目](https://leetcode-cn.com/submissions/detail/79168087/)
这个题很简单，直接贴代码，时间复杂度为$O(mn)$
``` c++
class Solution {
    public:
        string longestCommonPrefix(vector<string>& strs) {
            if (!strs.size()) {
                return "";
            }
            int length = strs[0].size();
            int count = strs.size();
            for (int i = 0; i < length; ++i) {
                char c = strs[0][i];
                for (int j = 1; j < count; ++j) {
                    if (i == strs[j].size() || strs[j][i] != c) {
                        return strs[0].substr(0, i);
                    }
                }
            }
            return strs[0];
        }
};
```
# 更优解
leetcode官方提供的解题思路中，有一种思路比较清奇，使用二分法来减少循环次数。显然，最长公共前缀长度不会超过最短字符串，那么就可以以此为依据开始二分查找，下面直接贴上leetcode官方代码
``` c++
class Solution {
    public:
        string longestCommonPrefix(vector<string>& strs) {
            if (!strs.size()) {
                return "";
            }
            int minLength = min_element(strs.begin(), strs.end(), [](const string& s, const string& t) {return s.size() < t.size();})->size();
            int low = 0, high = minLength;
            while (low < high) {
                int mid = (high - low + 1) / 2 + low;
                if (isCommonPrefix(strs, mid)) {
                    low = mid;
                }
                else {
                    high = mid - 1;
                }
            }
            return strs[0].substr(0, low);
        }

        bool isCommonPrefix(const vector<string>& strs, int length) {
            string str0 = strs[0].substr(0, length);
            int count = strs.size();
            for (int i = 1; i < count; ++i) {
                string str = strs[i];
                for (int j = 0; j < length; ++j) {
                    if (str0[j] != str[j]) {
                        return false;
                    }
                }
            }
            return true;
        }
};
```