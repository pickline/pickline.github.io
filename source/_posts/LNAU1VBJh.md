---
title: 'LeetCode 每日一题(1)'
date: 2020-06-11 16:40:26
tags: [leetcode]
---
# [题目](https://leetcode-cn.com/problems/daily-temperatures/)
题目链接在标题中，去掉包装在外面的话，题目的核心意思是寻找元素右边距离最近的更大元素。
# 暴力解法
最容易想到的解法是使用暴力法，双重循环遍历数组中的所有元素寻求解。
``` c++
class Solution {
    public:
        vector<int> dailyTemperatures(vecotr<int>& T) {
            int size = T.size();
            vector<int> res;
            for (int i = 0; i < size; i++) {
                for (int j = i + 1; j < size; j++) {
                    if (T[i] < T[j]) {
                        res[i] = j - i;
                        break;
                    }
                }
            }
            return res
        }
}
```
这是比较直观的一种解法，但是复杂度就比较不如人意了，显然，暴力法解题的复杂度是$O(mn)$。
# 简单解法
现在来介绍一下简单的解法。这个问题的核心在于找到最近的比自己大的元素，显然当紧邻的下一个元素不比自己大的时候，我们就无法直接得到答案。那么，为了降低时间复杂度，我们可以引入一个数据结构来记录已经遍历过的元素，这里我选择栈。
在一次遍历中，首先判断当前元素是否大于栈顶元素，如果大于栈顶元素，那么显然栈顶元素最近的比自己大的元素就是当前元素，计算结果之后，弹出栈顶元素，继续判断栈内元素直到栈空或者栈内元素比当前元素大，压入当前元素。下面贴出代码
``` c++
class Solution {
    public:
        vector<int> dailylTemperatures(vector<int>& T) {
            int size = T.size();
            vector<int> res;
            stack<int> st;
            for (int i = 0; i < size; i++) {
                while(!st.empty() && T[i] > T[st.top()]){
                    int previousIndex = st.top();
                    res[previousIndex] = i - previousIndex;
                    st.pop();
                }
            }
            st.push(i);
        }
}
```
