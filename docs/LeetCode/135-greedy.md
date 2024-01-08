 # Leetcode 135 Candy

https://leetcode.cn/problems/candy/description/

## 大常数算法：建图，求最长路

由于每个点的入度出度不超过 `2`，因此求最长路是 $O(n)$ 的。

所有入度为 `0` 的点都是没有限制条件的，因此可以直接分给他们 0 个糖果（不妨令所有人手里的糖果数 - 1）。

```cpp
// My Solution
const int MAXN = 2e4 + 3;
struct Edge{
    int node, val;
};
struct Node {
    vector<Edge> e;
    int in;
};

class Solution {
    Node edges[MAXN];
    void addEdge(int from, int to, int val) {
        if (from == to) return;
        edges[from].e.push_back((Edge){to, val});
        edges[to].in++;
    }
    void leftside(int i, int left, int midd) {
        if (left > midd) 
            addEdge(i, i - 1, 1);
    }
    void rightside(int i, int midd, int right) {
        if (right > midd)
            addEdge(i, i + 1, 1);
    }
    int dis[MAXN];

public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        for (int i = 0; i < n; i++)
            edges[i].in = 0;

        if (n > 1)
            rightside(0, ratings[0], ratings[1]);
        for (int i = 1; i < n - 1; i++) {
            leftside(i, ratings[i - 1], ratings[i]);
            rightside(i, ratings[i], ratings[i + 1]);
        }
        if (n > 1)
            leftside(n - 1, ratings[n - 2], ratings[n - 1]);     

        vector<int> zeroin;
        for (int i = 0; i < n; i++)
            if (edges[i].in == 0) {
                zeroin.push_back(i);
            }
        for (int i = 1; i < zeroin.size(); i++) {
            addEdge(zeroin[i], zeroin[i - 1], 0);
            addEdge(zeroin[i - 1], zeroin[i], 0);
        }
        addEdge(zeroin[0], zeroin[zeroin.size() - 1], 0);
        addEdge(zeroin[zeroin.size() - 1], zeroin[0], 0);

        std::queue<int> q;
        memset(dis, -1, sizeof(dis));
        for (int i = 0; i < zeroin.size(); i++) {
            dis[zeroin[i]] = 0; q.push(zeroin[i]);
        }
        while (!q.empty()) {
            auto node = q.front();
            q.pop();
            for (auto e : edges[node].e) {
                if (dis[e.node] < dis[node] + e.val) {
                    dis[e.node] = dis[node] + e.val;
                    q.push(e.node);
                }
            }
        }
        int sum = 0;
        for (int i = 0; i < n; i++)
            sum += dis[i];
        return sum + n;
    }
};
```

## 贪心

我们遍历该数组两次，处理出每一个学生分别满足左规则或右规则时，最少需要被分得的糖果数量。每个人最终分得的糖果数量即为这两个数量的最大值。

以左规则为例，如果递增，则糖果数 `+1`；如果递减，则糖果数 `=1`。

## 空间优化的贪心

注意到糖果总是尽量少给，且从 1 开始累计，每次要么比相邻的同学多给一个，要么重新置为 1。

我们从左到右枚举每一个同学，记前一个同学分得的糖果数量为 `pre`：

如果当前同学比上一个同学评分高，说明我们就在最近的递增序列中，直接分配给该同学 `pre+1` 个糖果即可。

否则我们就在一个递减序列中，我们直接分配给当前同学一个糖果，并把该同学所在的递减序列中所有的同学都再多分配一个糖果，以保证糖果数量还是满足条件。

我们无需显式地额外分配糖果，只需要记录当前的递减序列长度，即可知道需要额外分配的糖果数量。

递减序列一般无需包含峰值，但是要注意当当前的递减序列长度和上一个递增序列等长时，需要把最近的递增序列的最后一个同学也并进递减序列中（此时需要包含峰值）。

这样，我们只要记录当前递减序列的长度，最近的递增序列的长度，和前一个同学分得的糖果数量 `pre` 即可。

作者：力扣官方题解
链接：https://leetcode.cn/problems/candy/solutions/533150/fen-fa-tang-guo-by-leetcode-solution-f01p/
