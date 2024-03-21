# Leetcode 2386. 找出数组的第 K 大和

https://leetcode.cn/problems/find-the-k-sum-of-an-array/description/

## Code

$O(k^3)$, TLE. 

```cpp
struct Item { 
    long long sum;
    int maxind;
    bool index[2000 + 3];    
    Item() {
        sum = 0; maxind = -1; 
        memset(index, 0, sizeof(index));
    }
    bool operator < (const Item& rhs) {
        return sum < rhs.sum;
    }
};

class SortedList {
public:
    int size;
    vector<Item> vec;
    SortedList(int sz) : size(sz), vec() {
        Item item;
        vec.push_back(item);
    }
    void resort() {
        Item item = vec[vec.size() - 1];
        for (int i = vec.size() - 1; i > 0; i--) {
            if (item < vec[i - 1])
                vec[i] = vec[i - 1];
            else {
                vec[i] = item; break;
            }
        }
    }
    bool insert(Item& item) {
        if (vec.size() == size) {
            if (item < vec[size - 1])
                vec[size - 1] = item;
            else return false;
        }
        else vec.push_back(item);
        resort();
        return true;
    }
};

class Solution {
public:
    long long kSum(vector<int>& nums, int k) {
        long long sum = 0;
        vector<int> candidates;
        for (auto val : nums) {
            if (val < 0)
                candidates.push_back(-val);
            else {
                sum += val;
                candidates.push_back(val);
            }
        }
        sort(candidates.begin(), candidates.end());
        if (candidates.size() > k) candidates.resize(k);

        SortedList list(k);

        for (int i = 0; i < k; i++) {
            Item item = list.vec[i];
            for (int j = list.vec[i].maxind + 1; j < candidates.size(); j++) {
                if (item.index[j]) continue;
                item.index[j] = 1;
                item.sum += candidates[j];
                item.maxind = j;
                bool result = list.insert(item);
                item.sum -= candidates[j];
                item.index[j] = 0;
                item.maxind = list.vec[i].maxind;

                if (!result)
                    break;
            }
        }

        return sum - list.vec[k - 1].sum;
    }
};
```