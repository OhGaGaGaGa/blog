# Problem 7 - Reverse Integer - 整数反转

2022.01.17

> 给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。
>
> 如果反转后整数超过 32 位的有符号整数的范围 $[−2^{31},  2^{31} − 1]$ ，就返回 0。
>
> **假设环境不允许存储 64 位整数（有符号或无符号）。**

```
输入：x = 123
输出：321
```

## Code

许许多多小坑点。

```cpp
class Solution {
public:
    int reverse(int x) {
        if (x == 0x80000000) return 0;
        int ret = 0;
        bool positive = x >= 0 ? true : false;
        x = abs(x);
        while (x) {
            if (ret > 0x7fffffff / 10) return 0;
            ret *= 10;
            ret += x % 10;
            x /= 10;
        }
        if (!positive && ret == 0x80000000) return ret;
        if (ret < 0) return 0;
        return positive ? ret : -ret;
    }
};
```

