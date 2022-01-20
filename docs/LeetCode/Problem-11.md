# Problem 11 - Container With Most Water - 盛最多水的容器

2022.01.20

> 给你 $n$ 个非负整数 $a_1, a_2, \cdots, a_n$，每个数代表坐标中的一个点 $(i, a_i)$。在坐标内画 $n$ 条垂直线，垂直线 $i$ 的两个端点分别为 $(i, a_i)$ 和 $(i, 0)$。找出其中的两条线，使得它们与 $x$ 轴共同构成的容器可以容纳最多的水。
>

![img](https://gitee.com/ohg/typora_image/raw/master/imgs/2022/01/202201201120534.jpeg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

## $O(n\log n)$ - 单调栈 & 二分

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        vector<pair<int, int> > stk; // height, -index
        int ans = 0;
        for (int i = 0; i < height.size(); i++) {
            pair<int, int> pa = make_pair(height[i], -i);
            auto p = lower_bound(stk.begin(), stk.end(), pa);
            if (p == stk.end()) stk.push_back(pa);
            else ans = max(ans, height[i] * (i + (*p).second));
            for (auto it = stk.begin(); it != p && it != stk.end(); it++) {
                ans = max(ans, (*it).first * (i + (*it).second));
            }
        }
        return ans; 
    }
};
```

## $O(n)$ - 左挪右挪

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ans = 0;
        for (int i = 0, k = height.size() - 1; i < k;) {
            ans = max(ans, min(height[i], height[k])*(k-i));
            if (height[i] > height[k]) k--;
            else i++;
        }
        return ans;   
    }
};
```

