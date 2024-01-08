# 位运算及状压DP

参考：https://leetcode.cn/circle/discuss/CaOJ45/ 

## 从空集枚举到全集

$O(2^n)$

```cpp
for (int s = 0; s < (1 << n); s++) {
    // 处理 s 的逻辑
}
```

## 枚举某一集合 `s` 的**非空**子集 `sub`

```cpp
for (int sub = s; sub; sub = (sub - 1) & s) {
    // 处理 sub 的逻辑
}
```

暴力做法是从 `s` 出发，不断减 1。但是这样会有很多非子集情况，通过 `&s` 即可只保留有效的子集变化，将无效的借位给丢掉。

注意这是枚举的非空子集，空集仍需单独处理，或改为 `do-while`。

## 使用 Gosper's Hack，枚举恰好为 `k` 个元素的全集子集

https://programmingforinsomniacs.blogspot.com/2018/03/gospers-hack-explained.html

```cpp
int s = (1 << k) - 1; // init value
int limit = 1 << n;
while (s < limit) {
	// Do something with s
    
    int c = s & -s; // c is the last 1
    int r = s + c; // r set the last seq of 1 into 0, and set the near left 0 into 1
    s = (((r ^ s) / c) >> 2) | r; // get the changed bits, move to the right and remove 2 extra 1, then add back to r. 
}
```

2172. 数组的最大与和，题解
1125. 最小的必要团队，题解
2305. 公平分发饼干，题解
1494. 并行课程 II，题解
LCP 53. 守护太空城，题解
1879. 两个数组最小的异或值之和
1986. 完成任务的最少工作时间段

作者：灵茶山艾府
链接：https://leetcode.cn/circle/discuss/CaOJ45/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
