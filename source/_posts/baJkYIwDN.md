---
title: 'LeetCode 每日一题(3)'
date: 2020-06-15 10:14:21
tags: [leetcode]
---
# [题目](https://leetcode-cn.com/problems/powx-n/)
本日的题目引出了一个经典的算法———快速幂算法。
# 快速幂算法
快速幂算法的思想非常简单，即把整数的整数次幂分解为多个数的乘法。
a的b次幂可以转换成b次a的乘法
$$
    a ^ b = \underbrace{a * a * a ... * a}_{b}
$$
显然上面公式是最朴素的求解整数次幂的算法，算法的复杂度是$O(n)$。这个算法存在改进方法，即减少乘法次数。将幂数按照二进制展开
$$
    b = a_{0} * 2 ^ 0 + a_{1} * 2 ^ 1 + a_{2} * 2 ^ 2 + ... + a_{n} * 2 ^ n
$$
显然$n \leqslant Log_{2}{b}$，且
$$
    a = a ^ {a_{0} * 2 ^ 0 + a_{1} * 2 ^ 1 + a_{2} * 2 ^ 2 + ... + a_{n} * 2 ^ n} \\
    a =  a ^ {a_{0} * 2 ^ 0} + a ^ {a_{1} * 2 ^ 1}+ a ^ {a_{2} * 2 ^ 2} + ... + a ^ {a_{n} * 2 ^ n}
$$
这就是快速幂算法，时间复杂度为$O(Log_{2}n)$
# 代码
这是我写的解题代码
``` c++
class Solution {
    public:
        double myPow(double x, int n) {
            double res = 1.0;
            if (n < 0) {
                x = 1.0 / x;
            }
            while (n) {
                if (n & 1) {
                    res *= x; 
                }
                x *= x;
                n /= 2;
            }
            
            return res;
        }
};
```