 

# Leetcode 1553. Minimum Number of Days to Eat N Oranges

https://leetcode.cn/problems/minimum-number-of-days-to-eat-n-oranges/description/

直接记忆化搜索会 TLE。

可以发现，$-1$ 的次数在 $\div 2$ 之前最多 1 次，在 $\div 3$ 之前最多 2 次，因此可以进行剪枝。

对正整数 $n, x, y$，有
$$
\left\lfloor \frac{\lfloor n/x \rfloor}{y} \right\rfloor = \left\lfloor \frac{n}{xy} \right\rfloor = \left\lfloor \frac{\lfloor n/y \rfloor}{x} \right\rfloor
$$

实际上，只有所有满足 $i = \lfloor n/ (2^x3^y)\rfloor$ 的 `f[i]` 值才会被计算。

而 $x, y$ 都是 $\log n$ 的，因此需要计算的 `f[i]` 个数不超过 $\log^2 n$ 个。

因此时间空间复杂度均为 $O(\log^2 n)$。

应使用 `unordered_map` 来存储。

```cpp
// My Answer, Using Extra Space
const int MAXN = 1e7;

class Solution {
    int f[MAXN];
    int eat(int n) {
        if (n < MAXN && f[n] != -1) return f[n];
        int n2 = eat(n >> 1), n3 = eat(n / 3);
        int mn = std::min(n2 + n%2, n3 + n%3) + 1;
        if (n < MAXN) return f[n] = mn;
        else return mn;
    }
public:
    int minDays(int n) {
        memset(f, -1, sizeof(f));
        f[0] = 0;
        f[1] = 1;
        return eat(n);
    }
};
```

在发现上述性质后，可以对所有 $\lfloor n/2^x\rfloor$ 和 $\lfloor n/3^y\rfloor$ 作为节点建图，跑最短路，时间复杂度为 $O(\log^2 n\log\log n)$。

作者：力扣官方题解
链接：https://leetcode.cn/problems/minimum-number-of-days-to-eat-n-oranges/solutions/384947/chi-diao-n-ge-ju-zi-de-zui-shao-tian-shu-by-leetco/
