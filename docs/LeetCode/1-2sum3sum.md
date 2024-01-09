# Problem 1 & 15 - Two Sum & Three Sum

2022.01.16 & 2024.01.09

> 给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
>
> 你可以按任意顺序返回答案。

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

## $O(n^2)$

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (vector<int>::iterator it = nums.begin(); it != nums.end(); it++) {
            for (vector<int>::iterator jt = nums.begin(); jt != nums.end(); jt++) {
                if ((*it) + (*jt) == target && it != jt) {
                    vector<int> vec;
                    vec.push_back(it - nums.begin());
                    vec.push_back(jt - nums.begin());
                    return vec;
                }
            }
        }
        return vector<int>{};
    }
};
```

## $O(n\log n)$ - Sort & Lower Bound

```cpp
struct Unit {
    int value;
    int index;
};
class Solution {
public:
    static bool cmp(Unit a, Unit b) {
        return a.value < b.value;
    }
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<Unit> vec;
        for (auto it = nums.begin(); it != nums.end(); it++) {
            Unit u = {*it, static_cast<int>(it - nums.begin())};
            vec.push_back(u);
        }
        sort(vec.begin(), vec.end(), cmp);
        for (auto it = vec.begin(); it != vec.end(); it++) {
            auto bt = lower_bound(it + 1, vec.end(), (Unit){target - (*it).value, -1}, cmp);
            if (bt != vec.end() && (*it).value + (*bt).value == target) {
                return {(*it).index, (*bt).index};
            }
        }
        return {};
    }
};
```

## $O(n\log n + n)$ - Sort & Pointer

Used in [15. 3Sum](https://leetcode.cn/problems/3sum/). 

For a sorted array, binary search would use an extra $\log n$ time, so use the pointer instead. 

The additional code such as `continue` and `addLeft`, `subRight` are introduced due to the distinction constraint. 

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int size = nums.size();
        for (int i = 0; i < size - 2; i++) {
            if (nums[i] > 0) break;
            if (i >= 1 && nums[i] == nums[i - 1])
                continue;
            int j = i + 1, k = size - 1;
            int target = -nums[i];
            while (j < k) {
                int sum = nums[j] + nums[k];
                bool addLeft = 0, subRight = 0;
                if (sum == target) {
                    ans.push_back({nums[i], nums[j], nums[k]});
                    addLeft = 1; subRight = 1;
                }
                else if (sum < target) addLeft = 1;
                else subRight = 1;

                if (addLeft) {
                    do { j++; }
                    while (nums[j] == nums[j - 1] && j < k);
                }
                if (subRight) {
                    do { k--; }
                    while (nums[k] == nums[k + 1] && j < k);
                }
            }
        }
        return ans;
    }
};
// N^2
```

## $O(n)$ - Hash Map

```cpp
const int M = 19997;
class Solution {
public:
    // value, index
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<pair<int, int> > hashmap(M);
        for (int i = 0; i < M; i++) hashmap[i] = make_pair(1e9 + 1, -1);
        for (auto it = nums.begin(); it != nums.end(); it++) {
            int value = *it;
            int value_mod = ((value % M) + M) % M;
            int find = target - value;
            int find_mod = ((find % M) + M) % M;
            for (int i = find_mod; hashmap[i].first != 1e9 + 1; i++) {
                if (i == M) i = 0;
                if (hashmap[i].first + value == target) {
                    return {hashmap[i].second, static_cast<int>(it - nums.begin())};
                }
            } 
            int pos = value_mod;
            while (hashmap[pos].first != 1e9 + 1) {
                pos++;
                if (pos == M) pos = 0;
            }
            hashmap[pos] = make_pair(value, it - nums.begin());
        }
        return {};
    }
};
```

