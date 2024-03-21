# 子集划分

将一个集合分成两个尽可能相等的子集。

等价于 在集合中选择一些元素，使得其和尽可能接近但不超过 `sum/2`。

这是 NP Complete 问题。

对于贪心算法（从大到小排序后，依次放入 `sum` 较小的子集），是错误的，其近似比为 7/6。

反例：12 9 10 14 8 1

贪心解：
```
14 9 1  = 24
12 10 8 = 30
```

正解：
```
12 14 1 = 27
9 10 8  = 27
```

对于 Val 较小的，可以直接背包求解，时间复杂度为 $O(N\times \max_{1 \leq i \leq N} w_i)$。见

## Leetcode 416. Partition Equal Subset Sum

对于 Val 较大的，需要其他方法：

- **The Complete Greedy Algorithm (CGA)** considers all partitions by constructing a binary tree. Each level in the tree corresponds to an input number, where the root corresponds to the largest number, the level below to the next-largest number, etc. Each branch corresponds to a different set in which the current number can be put. Traversing the tree in DFS requires only $O(n)$ space, but might take $O(2^n)$ time. 
  - 启发式搜索：in each level, develop first the branch in which the current number is put in the set with the smallest sum. (greedy method)
  - 剪枝：若所有值为正，则如果把之后的所有元素都放入其中一个集合，两集合之差仍然比已有解更大，剪枝。
- **The Complete Karmarkar-Karp algorithm (CKK)** considers all partitions by constructing a binary tree. Each level corresponds to a pair of numbers. The left branch corresponds to putting them in different subsets (i.e., replacing them by their difference), and the right branch corresponds to putting them in the same subset (i.e., replacing them by their sum). This algorithm finds first the solution found by the largest differencing method, but then proceeds to find better solutions. It runs substantially faster than CGA on random instances. Its advantage is much larger when an equal partition exists, and can be of several orders of magnitude. In practice, problems of arbitrary size can be solved by CKK if the numbers have at most 12 significant digits. CKK can also run as an anytime algorithm: it finds the KK solution first, and then finds progressively better solutions as time allows (possibly requiring exponential time to reach optimality, for the worst instances).[1] It requires $O(n)$ space, and also in the worst case might take $O(2^n)$ time.

其中涉及到 KK solution：维护一个降序序列，取前两个作差后放入原序列，最后序列中剩的唯一一个数字即为 `sum1 - sum2`。作差的操作相当于在 `sum1` 中放入了较大的那个，在 `sum2` 中放入了较小的那个，这个操作引入了二者的差值。如果这个差值后续又作为减数，则相当于颠倒了顺序。这依然是贪心解，有 7/6 的近似比。

## Leetcode 2035. Partition Array Into Two Arrays to Minimize Sum Difference

Also see it at [牛客 KY231 第二题](https://www.nowcoder.com/practice/aea0458d54d74f3ca14012cbdf249918) 



KY231 Code Backup

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

class StringBuf {
public:
    string str;
    int ind;

    StringBuf(string s) : str(s), ind(0) {};
    char nxtchar() { 
        char output;
        if (ind < str.length()) output = str[ind]; 
        else output = EOF;
        ++ind;
        return output;
    }
    bool empty() { return ind >= str.length(); }
};

inline bool isspace(char c) {
    return c == ' ' || c == '\n' || c == '\r' || c == '\t';
}

int getNum(StringBuf& buf) {
    int ret = 0; char c = buf.nxtchar();
    while (c < '0' || c > '9') {
        if (c == EOF) return -1;
        if (!isspace(c)) return 0;
        c = buf.nxtchar();
    }
    while (c >= '0' && c <= '9') {
        ret = ret * 10 + c - '0';
        c = buf.nxtchar();
    }
    if (c == EOF || isspace(c)) return ret;
    return 0;
}

int main() {
    string line;
    vector<int> vec;
    vector<int> f;
    while (std::getline(cin, line)) {
        StringBuf buf(line);
        int a = getNum(buf);
        vec.clear();
        while (a > 0) {
            vec.push_back(a);
            a = getNum(buf);
            // cout << "a = " << a << endl;;
        }
        if (!a) {
            cout << "ERROR" << endl;
            continue;
        }

        int sum = 0;
        for (int i = 0; i < vec.size(); i++)
            sum += vec[i];
        int thres = sum / 2;
        
        f.clear();
        f.push_back(0);
        for (int i = 0; i < vec.size(); i++) {
            for 
        }
    }
     
}
```

一些题解：

https://blog.csdn.net/hh1986170901/article/details/105000680
https://cloud.tencent.com/developer/article/1930272
https://leetcode.cn/problems/partition-array-into-two-arrays-to-minimize-sum-difference/solutions/1097118/cong-ling-kai-shi-de-chun-ckun-nan-shua-fnohc/

## AcWing 3503

```cpp
#include <bits/stdc++.h>
using namespace std;

string input;

std::tuple<int, int> getint(int index) {
    int ret = 0; char ch = input[index++];
    while (ch < '0' || ch > '9') ch = input[index++];
    while (ch >= '0' && ch <= '9') {
        ret = ret * 10 + ch - '0'; ch = input[index++];
    }
    return std::make_tuple(ret, index);
}

vector<int> arr;
vector<int> s;
int target, diff;
bool ret = false;
void dfs(int index, int sum) { // sum is former
    if (sum > target) return;
    diff = min(diff, target - sum);
    if (diff == 0) return;
    if (index == arr.size()) return;
    if (target - (sum + s[index]) >= diff) return;
    if (sum + arr[index] > target) return;
    dfs(index + 1, sum + arr[index]);
    dfs(index + 1, sum);
}

int main() {
    getline(cin, input);
    int len = input.length();
    input += '*';
    input += '*';
    int index = 0;
    
    int sum = 0;
    while (index < len) {
        int num = 0;
        std::tie(num, index) = getint(index);
        arr.push_back(num);
        sum += num;
    }
    sort(arr.begin(), arr.end());
    s.resize(arr.size());
    s[arr.size() - 1] = arr[arr.size() - 1];
    for (int i = arr.size() - 2; i >= 0; i--)
        s[i] = s[i + 1] + arr[i];
    target = (sum + 1) / 2;
    diff = target;
    if (arr[arr.size() - 1] >= target)
        diff = target - (s[0] - arr[arr.size() - 1]);
    else dfs(0, 0);
    
    int x = target - diff, y = sum - (target - diff);
    if (x < y) x ^= y ^= x ^= y;
    cout << x << " " << y << endl;
    
}
```

1. 如果有一个特别大的（超过一半），那直接不用DFS了
2. 如果已经均分，那直接return
3. 如果后面的全都取仍然不会更优，剪
4. 在升序数组中，如果当前的加进去会超出一半，后面的肯定也会超，剪