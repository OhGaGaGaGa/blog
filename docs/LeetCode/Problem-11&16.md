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

# Problem 16 - 3Sum-Closest - 最接近的三数之和

2022.01.21

> 给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。
>
> 返回这三个数的和。
>
> 假定每组输入只存在恰好一个解。

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

 ## $O(n^2\log n)$ - 枚举2个 & 二分

使用 `upper_bound - 1` 找到小于等于 `target - nums[i] - nums[j]` 的最后一个数，使用 `lower_bound` 找到大于等于 `target - nums[i] - nums[j]` 的第一个数，然后更新答案。

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int minu = 100000;
        int sum = 0; 
        for (int i = 0; i < nums.size(); i++) {
            if (i && nums[i] == nums[i - 1]) 
                continue;
            for (int j = i + 2; j < nums.size(); j++) {
                if (j + 1 != nums.size() && nums[j] == nums[j + 1])
                    continue;
                int p = upper_bound(nums.begin() + i + 1, nums.begin() + j, target - nums[i] - nums[j]) - nums.begin() - 1;
                int q = lower_bound(nums.begin() + i + 1, nums.begin() + j, target - nums[i] - nums[j]) - nums.begin();
                if (p >= i + 1 && p < j) {
                    if (abs(target - nums[i] - nums[p] - nums[j]) < minu) {
                        minu = abs(target - nums[i] - nums[p] - nums[j]);
                        sum = nums[i] + nums[p] + nums[j];
                    }
                }
                if (q >= i + 1 && q < j) {
                    if (abs(target - nums[i] - nums[q] - nums[j]) < minu) {
                        minu = abs(target - nums[i] - nums[q] - nums[j]);
                        sum = nums[i] + nums[q] + nums[j];
                    }
                }
            }
        }
        return sum;
    }
};
```

## $O(n^2)$ - 两侧滑窗

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int res = 10000, left = 0, right = 0, temp = 0;
        for (int i = 0; i < nums.size() - 2; i++) {
            left = i + 1;
            right = nums.size() - 1;
            while (left < right) {
                temp = nums[i] + nums[left] + nums[right];
                if (temp == target)
                    return target;
                res = abs(temp - target) < abs(res - target) ? temp : res;
                if (temp - target > 0)
                    right--;
                else
                    left++;
            }
        }
        return res;
    }
};
```

