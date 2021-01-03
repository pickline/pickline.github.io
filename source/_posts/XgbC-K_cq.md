---
title: 'LeetCode 每日一题(2)'
date: 2020-06-13 16:50:55
tags: [leetcode]
---
# [题目](https://leetcode-cn.com/problems/climbing-stairs/)
看到这个题目，一开始没啥思路，后面在纸上简单的推了6、7次之后，发现得出的一系列值符合斐波那契数列的特征。
# 递归法
在知道是斐波那契数列之后，我第一时间想到的就是使用递归来得出结果。
``` c
int climbStairs(int n) {
    if (n == 1 || n == 2) {
        return n;
    }else {
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```
这种方法非常直观，但是效率比较低，而且递归栈也会特别深，有移除的风险
# 改良的递归
这里使用vector来记录下之前的结果，就可以避免递归会出现的问题
``` c++
class Solution {
    public:
        vector<int> fab;
        int climbStairs(int n) {
            if (n == 1 || n == 2) {
                this->fab.push_back(n);
            }else {
                this->fab.push_back(this->fab[n - 2] + this->fab[n - 3]);
            }
            return this->fab[n - 1];
        }
};
```
# 优秀解法
实际上这里的问题只关注最终的值，并不关心中间结果，而斐波那契数列第n项的值只依赖于前面两项，因此我们完全可以只记录3项的值，这样时间复杂度仅为$O(n)$，而空间复杂度也只是$O(1)$，算是比较优越的算法。
``` c
int climbStairs(int n) {
    int p, q = 0;
    int r = 1;
    for (int i = 0; i < n; i++) {
        p = q;
        q = r;
        r = q + p;
    }
    return r;
}
```